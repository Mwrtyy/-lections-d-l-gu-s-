<script>
  import { onMount } from "svelte";
  import gsap from "gsap";
  import ScrollTrigger from "gsap/ScrollTrigger";

  const paragraph = "Vous parlez. On écoute. On transmet. On revient avec des nouvelles.";
  const words = paragraph.split(" ");
  let manifesto = $state();

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger);
    if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) return;
    const spans = manifesto.querySelectorAll(".word");
    gsap.set(spans, { color: "rgba(23, 23, 21, .16)" });
    const reveal = gsap.to(spans, {
      color: "#171715",
      stagger: 0.12,
      ease: "none",
      scrollTrigger: { trigger: manifesto, start: "top 76%", end: "bottom 48%", scrub: 1 }
    });
    return () => reveal.scrollTrigger?.kill();
  });
</script>

<section id="manifeste" class="manifesto">
  <div class="container manifesto-inner">
    <span class="eyebrow manifesto-label"><span class="eyebrow-mark"></span> Le principe est simple</span>
    <p bind:this={manifesto} class="manifesto-words" aria-label={paragraph}>
      {#each words as word}<span aria-hidden="true" class="word">{word}</span>{/each}
    </p>
    <div class="manifesto-note"><span class="manifesto-rule"></span>
      <p>Pas de grand discours. On prend la parole pour que vous n’ayez pas à la reprendre deux fois.</p>
    </div>
  </div>
</section>
