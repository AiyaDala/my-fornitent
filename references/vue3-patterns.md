# Vue 3 Composition API Patterns

## Component structure

Always use `<script setup lang="ts">`. It's the standard for Vue 3 + TypeScript.

### Standard component template

```vue
<script setup lang="ts">
// 1. Imports
import { ref, computed, watch, onMounted } from 'vue'
import type { PropType } from 'vue'
import { useDebounceFn } from '@vueuse/core'
import MyChildComponent from './MyChildComponent.vue'

// 2. Types & Interfaces
export interface Item {
  id: string | number
  name: string
  // ...
}

// 3. Props with full TypeScript
const props = defineProps({
  items: {
    type: Array as PropType<Item[]>,
    required: true,
  },
  loading: {
    type: Boolean,
    default: false,
  },
  size: {
    type: String as PropType<'sm' | 'md' | 'lg'>,
    default: 'md',
    validator: (v: string) => ['sm', 'md', 'lg'].includes(v),
  },
})

// 4. Emits — always type the payload
const emit = defineEmits<{
  (e: 'select', item: Item): void
  (e: 'delete', id: string | number): void
  (e: 'update:page', page: number): void
}>()

// 5. Composables (extracted reusable logic)
const { data, error, refresh } = useFetch('/api/items')

// 6. Local reactive state
const selectedId = ref<string | number | null>(null)
const searchQuery = ref('')
const isOpen = ref(false)

// 7. Computed — derived state, keep pure (no side effects)
const filteredItems = computed(() =>
  props.items.filter(item =>
    item.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
)

const hasSelection = computed(() => selectedId.value !== null)

// 8. Methods — event handlers and actions
function handleSelect(item: Item) {
  selectedId.value = item.id
  emit('select', item)
}

function handleDelete(id: string | number) {
  if (confirm('确定删除？')) {
    emit('delete', id)
  }
}

const debouncedSearch = useDebounceFn((query: string) => {
  // API call or complex filtering
}, 300)

// 9. Lifecycle hooks
onMounted(() => {
  // DOM available, fetch initial data
})

// 10. Watchers — for side effects that react to state changes
watch(searchQuery, (newQuery) => {
  debouncedSearch(newQuery)
})
</script>

<template>
  <!-- template here -->
</template>

<style scoped>
/* component styles */
</style>
```

## Props best practices

### Do: Use specific types
```typescript
const props = defineProps({
  status: {
    type: String as PropType<'idle' | 'loading' | 'success' | 'error'>,
    required: true,
  },
  count: {
    type: Number,
    default: 0,
  },
})
```

### Don't: Use Object as a catch-all
```typescript
// BAD
const props = defineProps({ config: Object })

// GOOD
interface Config {
  showHeader: boolean
  pageSize: number
}
const props = defineProps({
  config: {
    type: Object as PropType<Config>,
    required: true,
  },
})
```

### v-model support

For form components, support `v-model`:

```typescript
// Single v-model
const props = defineProps({ modelValue: { type: String, required: true } })
const emit = defineEmits<{ (e: 'update:modelValue', value: string): void }>()

// Multiple v-models (v-model:title, v-model:content)
const props = defineProps({
  title: { type: String, required: true },
  content: { type: String, required: true },
})
const emit = defineEmits<{
  (e: 'update:title', value: string): void
  (e: 'update:content', value: string): void
}>()
```

## Composables pattern

Extract reusable stateful logic into composables. A composable is a function that
returns reactive state and methods.

```typescript
// composables/useList.ts
import { ref, computed } from 'vue'
import type { Ref, ComputedRef } from 'vue'

interface UseListOptions<T> {
  fetchFn: () => Promise<T[]>
  initialData?: T[]
}

interface UseListReturn<T> {
  items: Ref<T[]>
  loading: Ref<boolean>
  error: Ref<Error | null>
  isEmpty: ComputedRef<boolean>
  refresh: () => Promise<void>
}

export function useList<T>(options: UseListOptions<T>): UseListReturn<T> {
  const items = ref<T[]>(options.initialData ?? []) as Ref<T[]>
  const loading = ref(false)
  const error = ref<Error | null>(null)

  const isEmpty = computed(() => !loading.value && items.value.length === 0)

  async function refresh() {
    loading.value = true
    error.value = null
    try {
      items.value = await options.fetchFn()
    } catch (e) {
      error.value = e as Error
    } finally {
      loading.value = false
    }
  }

  return { items, loading, error, isEmpty, refresh }
}
```

### Naming conventions
- Composable files: `use[Feature].ts`
- Composable functions: `use[Feature]()`
- Return an object (not an array) so consumers can destructure by name

## Slots usage

Use slots for flexible content, not prop-driven content:

```vue
<!-- DO: Slots for content that varies -->
<template>
  <div class="card">
    <div class="card-header">
      <slot name="header">
        <h3 v-if="title">{{ title }}</h3>
      </slot>
    </div>
    <div class="card-body">
      <slot />
    </div>
    <div v-if="$slots.footer" class="card-footer">
      <slot name="footer" />
    </div>
  </div>
</template>

<!-- DON'T: Props for complex content -->
<!-- <Card title="..." body="..." footer="..." /> -->
```

### Slot props for renderless patterns

```vue
<!-- Parent: List component exposes item data via slot props -->
<template>
  <ul>
    <li v-for="item in items" :key="item.id">
      <slot name="item" :item="item" :index="index" :active="item.id === activeId" />
    </li>
  </ul>
</template>

<!-- Consumer -->
<DataList :items="users">
  <template #item="{ item, active }">
    <UserCard :user="item" :class="{ highlighted: active }" />
  </template>
</DataList>
```

## Common anti-patterns

### Don't mutate props
```typescript
// BAD: mutating a prop
props.items.push(newItem)

// GOOD: emit an event, let the parent mutate
emit('add', newItem)

// GOOD: create a local copy if you need to mutate locally
const localItems = ref([...props.items])
```

### Don't use $ref / $computed
Stay with `ref()` and `computed()` from the Composition API. `$ref` is a reactivity
transform that's no longer recommended.

### Don't over-use watchers
```typescript
// BAD: watcher that could be a computed
const fullName = ref('')
watch([firstName, lastName], ([f, l]) => {
  fullName.value = `${f} ${l}`
})

// GOOD: just use computed
const fullName = computed(() => `${firstName.value} ${lastName.value}`)
```

### Don't use index as key with mutable lists
```vue
<!-- BAD: key is index -->
<li v-for="(item, index) in items" :key="index">

<!-- GOOD: key is stable unique id -->
<li v-for="item in items" :key="item.id">
```

## Template conventions

- Use `v-if` + `v-else` chains for mutually exclusive states (not multiple `v-if`s)
- Use `v-show` for frequent toggles; `v-if` for rare toggles
- Use `<template>` for grouping without extra DOM nodes
- Kebab-case component tags: `<user-card />` not `<UserCard />`
- Self-close void components: `<UserCard />` not `<UserCard></UserCard>`

## Directive usage

```vue
<!-- Conditional rendering -->
<div v-if="isVisible">Content</div>

<!-- List rendering with key -->
<div v-for="item in items" :key="item.id">{{ item.name }}</div>

<!-- Class binding -->
<div :class="['base-class', { active: isActive, disabled: !canEdit }]">

<!-- Style binding (prefer CSS classes over inline styles) -->
<div :style="{ height: `${rowHeight}px` }">

<!-- Event handlers -->
<button @click="handleClick" @keydown.enter="handleSubmit">
```

## Form handling

```vue
<script setup lang="ts">
import { reactive, ref } from 'vue'

interface FormData {
  name: string
  email: string
  role: 'admin' | 'user'
}

interface FormErrors {
  name?: string
  email?: string
  role?: string
}

const form = reactive<FormData>({
  name: '',
  email: '',
  role: 'user',
})

const errors = reactive<FormErrors>({})
const submitting = ref(false)

function validate(): boolean {
  // Clear previous errors
  Object.keys(errors).forEach(k => delete errors[k as keyof FormErrors])

  if (!form.name.trim()) errors.name = '请输入名称'
  if (!form.email.includes('@')) errors.email = '请输入有效的邮箱'
  if (!['admin', 'user'].includes(form.role)) errors.role = '请选择角色'

  return Object.keys(errors).length === 0
}

async function handleSubmit() {
  if (!validate()) return
  submitting.value = true
  try {
    await api.submit(form)
    // success handling
  } catch (e) {
    // error handling
  } finally {
    submitting.value = false
  }
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <div class="form-field" :class="{ error: errors.name }">
      <label for="name">名称</label>
      <input id="name" v-model="form.name" type="text" />
      <span v-if="errors.name" class="error-message" role="alert">{{ errors.name }}</span>
    </div>
    <!-- more fields -->
    <button type="submit" :disabled="submitting">
      {{ submitting ? '提交中...' : '提交' }}
    </button>
  </form>
</template>
```

## Using VueUse

Prefer `@vueuse/core` utilities over writing your own:
- `useDebounceFn` / `useThrottleFn` for rate limiting
- `useMediaQuery` for responsive logic
- `useLocalStorage` / `useSessionStorage` for persistent state
- `useElementSize` / `useWindowSize` for dimension-aware components
- `useIntersectionObserver` for scroll-triggered loading
- `onClickOutside` for dropdown/panel dismissal
- `useEventListener` for clean event binding
