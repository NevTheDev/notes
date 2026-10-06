# Vue

## Creating an app

`npm create vue@latest`

## Components

```html
<script setup lang="ts"></script>

<template>--Component Html Here</template>

<style scoped lang="scss"></style>
```

### Lifecycle Hooks

```ts
// Called after all sync children have been mounted and its own dom tree has been created and inserted into the parent container.
onMounted(() => {});

// Called after the component is updated due to reactive state change.
onUpdated(() => {});

// Called when all child components have been unmounted all reactive effects have been stopped.
onUnmounted(() => {});

// Called when setup is complete but no DOM created
onBeforeMount(() => {});

// Called before the component is updates the DOM
onBeforeUpdate(() => {});

// Called before the component is unmounted
onBeforeUnmount(() => {});
```

## Reactivity

```ts
const refValue = ref("");

refValue.value;
refValue.value = "Hello World";

// Is computed when the ref changes

const computedValue = computed(() => {
  return refValue.value + " Amount";
});

computedValue.value;

//
const reactiveValue = reactive({});
```

### Props

```ts
const props = defineProps(["title"]);

// or

const { title } = defineProps(["title"]);
```

When using typescript we can define a type or interface that defines the props to be passed in

```ts
type MyProps = {
  title: string;
};

const props = defineProps<MyProps>();

// or

const { title } = defineProps<MyProps>();
```

```html
<MyComponent title="This is the title" />

// Binding to a value in the parent
<MyComponent :title="somevalue" />
```

### Events/Emitters

```ts
const emit = defineEmits(["change"]);

// Call the emit function
emit("change");
```

```html
<MyComponent @change="() => console.log(bla)" />

// Binding to a value in the parent
<MyComponent @change="SomeMethod" />
```

## Routing

### Defining routes

```ts
import { createRouter, createWebHistory } from "vue-router";
import HomeView from "@/features/home/HomeView.vue";

const routes = [
  {
    path: "/",
    name: "home",
    component: HomeView,
  },
  {
    path: "/dashboard",
    name: "about",
    component: () => import("../features/dashboard/DashboardView.vue"), // Lazy load component
  },
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes,
});

export default router;
```

### Dynamic Route Values

```ts
const routes = [
    ...
    {
        path: '/product/:productId',
        ...
    },
]
```

```ts
import { useRoute } from "vue-router";

const route = useRoute();
const productId = route.params.productId;
```
