<script setup lang="ts">
import type { PropType } from "vue";

defineProps({
  options: {
    type: Object as PropType<Options>,
    required: true,
  },
});

const navbar = useNavbarStore();
const isLarge = inject("isLarge") as Ref<boolean>;

type Options = {
  name: string;
  options: {
    name: string;
    icon: string;
    to: string;
    isMobile?: boolean;
  }[];
};
</script>

<template>
  <div class="flex flex-col gap-4">
    <div class="w-full border-t border-slate-800"></div>
    <h4
      class="text-sm font-bold uppercase text-gray-400"
      :class="{
        'lg:hidden': !navbar.isOpen,
      }"
    >
      {{ options.name }}
    </h4>
    <div
      class="flex flex-col gap-3"
      :class="{
        'pl-3': navbar.isOpen,
      }"
    >
      <NuxtLink
        v-for="option in options.options"
        :key="option.name"
        class="flex gap-3 items-center font-semibold"
        :class="{
          'md:hidden': option.isMobile,
        }"
        :to="option.to"
      >
        <template v-if="navbar.isOpen || !isLarge">
          <Icon :name="option.icon" />
          <span class="text-sm">
            {{ option.name }}
          </span>
        </template>
        <template v-else>
          <div
            class="w-[50px] h-[50px] rounded-lg flex justify-center items-center hover:bg-slate-900/30 cursor-pointer transition-colors duration-300"
            :class="{
              'md:hidden': option.isMobile,
            }"
          >
            <Icon :name="option.icon" class="text-3xl" />
          </div>
        </template>
      </NuxtLink>
    </div>
  </div>
</template>
