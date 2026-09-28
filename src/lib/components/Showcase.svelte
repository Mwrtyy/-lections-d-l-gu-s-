<script>
  import { onMount } from "svelte";
  import gsap from "gsap";
  import ScrollTrigger from "gsap/ScrollTrigger";

  const commitments = [
    { time: "AVANT LE CONSEIL", title: "On vous demande ce qui compte.", description: "Avant chaque conseil de classe, on consulte TG4 pour faire remonter les sujets qui concernent le groupe.", mark: "01" },
    { time: "PENDANT", title: "Vos mots, sans les déformer.", description: "On transmet vos demandes et difficultés aux professeurs et à la vie scolaire, clairement et fidèlement.", mark: "02" },
    { time: "APRÈS", title: "On revient avec les réponses.", description: "Après les échanges, on fait un retour utile à la classe, en respectant la confidentialité des situations personnelles.", mark: "03" },
    { time: "TOUTE L’ANNÉE", title: "Personne ne reste hors boucle.", description: "On fait circuler les informations utiles à tout le monde. Oui, jusqu’au fond de la salle.", mark: "04" }
  ];
  let grid = $state();

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger);
    if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) return;
    const cards = grid.querySelectorAll(".service-grid-card");
    gsap.fromTo(cards, { y: 56, autoAlpha: 0 }, {
      y: 0, autoAlpha: 1, duration: 0.95, stagger: 0.12, ease: "expo.out",
      scrollTrigger: { trigger: grid, start: "top 82%" }
    });
    cards.forEach((card, index) => {
      if (index === cards.length - 1) return;
      gsap.to(card, {
        scale: 0.975, opacity: 0.74, ease: "none",
        scrollTrigger: { trigger: cards[index + 1], start: "top 85%", end: "top 26%", scrub: true }
      });
    });
    return () => ScrollTrigger.getAll().forEach((trigger) => {
      if (grid.contains(trigger.trigger)) trigger.kill();
    });
  });
</script>

<section id="engagements" class="commitments">
  <div class="container commitments-head">
    <div><span class="eyebrow"><span class="eyebrow-mark"></span> Quatre engagements</span>
      <h2>Moins de promesses.<br /><em>Plus de suivi.</em></h2></div>
    <p>Le rôle est simple : écouter la classe, transmettre, puis revenir avec des nouvelles.</p>
  </div>
  <div class="container commitment-stack" bind:this={grid}>
    {#each commitments as item, i}
      <article class="service-grid-card commitment-card card-{i + 1}">
        <div class="commitment-top"><span class="commitment-index">{item.mark}</span>
          <span class="commitment-time">{item.time}</span><span class="commitment-dot" aria-hidden="true">↗</span></div>
        <div class="commitment-content"><h3>{item.title}</h3><p>{item.description}</p></div>
        <div class="commitment-bottom"><span>KAÏS × MICHEL</span><span>TG4 · 2026</span></div>
      </article>
    {/each}
  </div>
</section>
