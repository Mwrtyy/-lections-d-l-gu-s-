<script>
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import Lenis from "lenis";
  import "lenis/dist/lenis.css";
  import "./layout.css";
  import favicon from "$lib/assets/favicon.svg";
  import { setLenis } from "$lib/scroll.js";
  import Header from "$lib/components/header.svelte";
  let { children } = $props();

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger);
    if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) return;
    const lenis = new Lenis({ autoRaf: false, duration: 1.05, easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)), smoothWheel: true, syncTouch: false });
    const raf = (time) => lenis.raf(time * 1000);
    setLenis(lenis);
    lenis.on("scroll", ScrollTrigger.update);
    gsap.ticker.add(raf);
    gsap.ticker.lagSmoothing(0);
    return () => { lenis.destroy(); setLenis(null); gsap.ticker.remove(raf); };
  });
</script>

<svelte:head>
  <link rel="icon" href={favicon} />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
</svelte:head>

<Header />
<main class="app-wrapper">{@render children()}</main>
