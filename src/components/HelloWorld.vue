<script setup lang="ts">
import { computed, nextTick, reactive, ref, toRef, toRefs, watch, type ToRefs } from 'vue';

const props = defineProps<{
  msg: string;
  parentCount: number;
  modelValue: string;
}>();

const emits = defineEmits<{ (message: "update:modelValue", text: string): void }>()

console.log(`props.msg: ${props.msg}`);
console.log(`props.parentCount: ${props.parentCount}`);

// type Obj = { [key: string]: any };
//
// const reactiveWithRefs = <T extends Record<string, unknown>>(obj: T): ToRefs<T> => {
//   return toRefs(reactive(obj))
// }
// const obj1 = reactiveWithRefs({ count: 1 })

// const obj = toRefs(reactive({
//   c: 1
// }))

// const obj = reactive({
//   count: 1
// })
//
//
// let { count } = obj
//
// const increment = () => {
//   count++;
// }

// const obj = toRefs(
//   reactive({
//     count: 1,
//   })
// );
//
// const increment = () => {
//   obj.count.value++
// }

const count = ref(0);

const increment = async () => {
  count.value++
  console.log("before DOM rendering");
  console.log(document.querySelector("#counter")?.textContent)
  await nextTick()
  console.log("after DOM rendering");
  console.log(document.querySelector("#counter")?.textContent)
}

const incrementWithParentCount = computed(() => {
  console.log(`count.value: ${count.value}`);
  return count.value + props.parentCount;
})

watch(count, (newCount, oldCount) => {
  console.log(`newCount: ${newCount}, oldCount: ${oldCount}`);
});

const onInputText = (event: Event) => {
  if (!event.target) {
    return;
  }

  if (!isHTMLInputElement(event.target)) {
    return;
  }

  emits("update:modelValue", event.target.value)
}

const isHTMLInputElement = (target: EventTarget): target is HTMLInputElement => {
  return target instanceof HTMLInputElement
}
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h1 id="counter" class="green">{{ incrementWithParentCount }}</h1>
    <button @click="increment">+</button>
    <input :value="modelValue" @input="onInputText" />
    <slot name="hogeSlot"></slot>
    <h3>
      You’ve successfully created a project with
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
      What's next?
    </h3>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
