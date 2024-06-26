<script setup lang="ts">
// @ts-ignore
import { useSound } from "@vueuse/sound";
import lightOn from "public/sounds/light-on.mp3";
import lightOff from "public/sounds/light-off.mp3";
const { play: playLight } = useSound(lightOn, { volume: 0.5 });
const { play: playDark } = useSound(lightOff, { volume: 0.5 });

const isDark = useDark({
  attribute: "data-theme",
});
const toggleDark = useToggle(isDark);

const darkMode = ref(isDark.value);

const switchTheme = (event: MouseEvent) => {
  const isAppearanceTransition =
    document.startViewTransition &&
    !window.matchMedia("(prefers-reduced-motion: reduce)").matches;
  if (!isAppearanceTransition || !event) {
    toggleDark();
    return;
  }
  const x = event.clientX;
  const y = event.clientY;
  const endRadius = Math.hypot(
    Math.max(x, innerWidth - x),
    Math.max(y, innerHeight - y)
  );
  // @ts-expect-error: Transition API
  const transition = document.startViewTransition(() => {
    toggleDark();
  });
  transition.ready.then(() => {
    isDark.value ? playDark() : playLight();
    const clipPath = [
      `circle(0px at ${x}px ${y}px)`,
      `circle(${endRadius}px at ${x}px ${y}px)`,
    ];
    document.documentElement.animate(
      {
        clipPath: isDark.value ? [...clipPath].reverse() : clipPath,
      },
      {
        duration: 400,
        easing: "ease-in",
        pseudoElement: isDark.value
          ? "::view-transition-old(root)"
          : "::view-transition-new(root)",
      }
    );
  });
};
</script>

<template>
  <label
    class="swap swap-rotate w-[2.5rem] h-[2.5rem] bg-opacity-80 hover:scale-[1.15] active:scale-105 transition-all"
  >
    <input
      type="checkbox"
      class="theme-controller"
      v-model="darkMode"
      @click.stop="switchTheme"
    />

    <!-- sun icon -->
    <svg
      class="swap-off fill-current w-4 h-4"
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
    >
      <path
        d="M5.64,17l-.71.71a1,1,0,0,0,0,1.41,1,1,0,0,0,1.41,0l.71-.71A1,1,0,0,0,5.64,17ZM5,12a1,1,0,0,0-1-1H3a1,1,0,0,0,0,2H4A1,1,0,0,0,5,12Zm7-7a1,1,0,0,0,1-1V3a1,1,0,0,0-2,0V4A1,1,0,0,0,12,5ZM5.64,7.05a1,1,0,0,0,.7.29,1,1,0,0,0,.71-.29,1,1,0,0,0,0-1.41l-.71-.71A1,1,0,0,0,4.93,6.34Zm12,.29a1,1,0,0,0,.7-.29l.71-.71a1,1,0,1,0-1.41-1.41L17,5.64a1,1,0,0,0,0,1.41A1,1,0,0,0,17.66,7.34ZM21,11H20a1,1,0,0,0,0,2h1a1,1,0,0,0,0-2Zm-9,8a1,1,0,0,0-1,1v1a1,1,0,0,0,2,0V20A1,1,0,0,0,12,19ZM18.36,17A1,1,0,0,0,17,18.36l.71.71a1,1,0,0,0,1.41,0,1,1,0,0,0,0-1.41ZM12,6.5A5.5,5.5,0,1,0,17.5,12,5.51,5.51,0,0,0,12,6.5Zm0,9A3.5,3.5,0,1,1,15.5,12,3.5,3.5,0,0,1,12,15.5Z"
      />
    </svg>

    <!-- moon icon -->
    <svg
      class="swap-on fill-current w-4 h-4"
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
    >
      <path
        d="M21.64,13a1,1,0,0,0-1.05-.14,8.05,8.05,0,0,1-3.37.73A8.15,8.15,0,0,1,9.08,5.49a8.59,8.59,0,0,1,.25-2A1,1,0,0,0,8,2.36,10.14,10.14,0,1,0,22,14.05,1,1,0,0,0,21.64,13Zm-9.5,6.69A8.14,8.14,0,0,1,7.08,5.22v.27A10.15,10.15,0,0,0,17.22,15.63a9.79,9.79,0,0,0,2.1-.22A8.11,8.11,0,0,1,12.14,19.73Z"
      />
    </svg>
  </label>
</template>

<style>
::view-transition-old(root),
::view-transition-new(root) {
  animation: none;
  mix-blend-mode: normal;
}

::view-transition-old(root) {
  z-index: 1;
}

::view-transition-new(root) {
  z-index: 2147483646;
}

[data-theme="dark"]::view-transition-old(root) {
  z-index: 2147483646;
}

[data-theme="dark"]::view-transition-new(root) {
  z-index: 1;
}
</style>
