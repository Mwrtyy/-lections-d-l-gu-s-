<script>
  import { onMount } from "svelte";
  import gsap from "gsap";

  let firstLine = $state();
  let secondLine = $state();
  let details = $state();
  let actions = $state();
  let artwork = $state();
  let disc = $state();

  onMount(() => {
    if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) return;
    const lines = [firstLine, secondLine];
    const intro = gsap.timeline({ delay: 0.12 });
    gsap.set(artwork, { autoAlpha: 0, scale: 0.92, rotate: -8 });
    gsap.set(lines, { yPercent: 115, rotateZ: 2 });
    gsap.set([details, actions], { y: 20, autoAlpha: 0 });
    intro.to(artwork, { autoAlpha: 1, scale: 1, rotate: 0, duration: 1.4, ease: "power3.out" })
      .to(lines, { yPercent: 0, rotateZ: 0, duration: 1.15, stagger: 0.12, ease: "expo.out" }, "-=0.95")
      .to(details, { y: 0, autoAlpha: 1, duration: 0.8, ease: "power3.out" }, "-=0.55")
      .to(actions, { y: 0, autoAlpha: 1, duration: 0.7, ease: "power3.out" }, "-=0.45");

    if (!window.matchMedia("(hover: hover) and (pointer: fine)").matches) return () => intro.kill();
    const move = (event) => {
      const x = (event.clientX / innerWidth - 0.5) * 12;
      const y = (event.clientY / innerHeight - 0.5) * 12;
      gsap.to(artwork, { x: -x * 0.5, y: -y * 0.5, duration: 1.5, ease: "power2.out" });
      gsap.to(disc, { x, y, duration: 1.35, ease: "power2.out" });
    };
    window.addEventListener("pointermove", move);
    return () => { window.removeEventListener("pointermove", move); intro.kill(); };
  });
</script>

<section id="accueil" class="hero">
  <div class="hero-grid" aria-hidden="true"></div>
  <div class="container hero-layout">
    <div class="hero-copy">
      <p class="eyebrow"><span class="eyebrow-mark"></span> Élection des délégués · Terminale générale 4</p>
      <h1>
        <span class="hero-line"><span bind:this={firstLine}>Deux voix</span></span>
        <span class="hero-line hero-line-serif"><span bind:this={secondLine}>pour la vôtre.</span></span>
      </h1>
      <p class="hero-subtitle" bind:this={details}>
        Kaïs BATTAIA PISZCZ <span class="cross">×</span> Michel Kostenko<br />
        <strong>Le vote, c’est aujourd’hui.</strong> 28 septembre 2026.
      </p>
      <div class="hero-actions" bind:this={actions}>
        <a class="button button-dark" href="#binome">Découvrir le binôme <span aria-hidden="true">↘</span></a>
        <a class="text-link" href="#engagements">Nos quatre engagements</a>
      </div>
      <p class="hero-footnote">Oui, même le fond de la salle.</p>
    </div>

    <div class="hero-art" bind:this={artwork} aria-hidden="true">
      <div class="hero-art-halo"></div>
      <div class="hero-art-disc" bind:this={disc}>
        <div class="disc-top"><span>TG4</span><span>28 · 09 · 26</span></div>
        <div class="disc-center"><span>ÉLUS</span><span class="ampersand">&amp;</span><span>À L’ÉCOUTE</span></div>
        <div class="disc-bottom"><span>KAÏS + MICHEL</span><span>← DEUX VOIX →</span></div>
      </div>
      <div class="hero-art-tag">Pas un sondage.<br />Une vraie élection.</div>
      <span class="hero-spark spark-one">✳</span><span class="hero-spark spark-two">+</span>
    </div>
  </div>
  <div class="hero-bottom container">
    <span>28 SEPTEMBRE 2026</span><span>KAÏS &amp; MICHEL · TG4</span>
    <a href="#manifeste">Faire défiler <span aria-hidden="true">↓</span></a>
  </div>
</section>
