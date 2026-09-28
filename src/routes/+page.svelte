<script>
  import "./campaign.css";
  import Seo from "$lib/components/Seo.svelte";
  import Hero from "$lib/components/Hero.svelte";
  import Manifesto from "$lib/components/Manifesto.svelte";
  import Showcase from "$lib/components/Showcase.svelte";

  const candidates = [
    { first: "Kaïs", last: "BATTAIA PISZCZ", role: "Candidat · TG4", mark: "K" },
    { first: "Michel", last: "Kostenko", role: "Candidat · TG4", mark: "M" }
  ];
</script>

<Seo />

<div class="campaign">
  <Hero />
  <Manifesto />
  <Showcase />

  <section id="binome" class="pair-section">
    <div class="container pair-layout">
      <div class="pair-heading">
        <span class="eyebrow"><span class="eyebrow-mark"></span> Le binôme</span>
        <h2>Deux personnalités.<br /><em>Une même écoute.</em></h2>
        <p>Une classe entière à représenter. Du premier rang au fond de la salle.</p>
      </div>
      <div class="candidate-list">
        {#each candidates as person, i}
          <article class="candidate">
            <span class="candidate-index">0{i + 1}</span>
            <div class="candidate-name"><span>{person.first}</span><strong>{person.last}</strong></div>
            <span class="candidate-role">{person.role}</span>
            <span class="candidate-mark" aria-hidden="true">{person.mark}</span>
          </article>
        {/each}
      </div>
    </div>
    <div class="pair-bottom container"><span>KAÏS BATTAIA PISZCZ × MICHEL KOSTENKO</span><span>TERMINALE GÉNÉRALE 4</span></div>
  </section>

  <section id="vote" class="finale">
    <div class="finale-shape" aria-hidden="true"><span>28</span><i>09</i><b>26</b></div>
    <div class="container finale-inner">
      <span class="eyebrow finale-eyebrow"><span class="eyebrow-mark"></span> 28 septembre 2026 · Aujourd’hui</span>
      <h2>Le vote,<br /><em>c’est aujourd’hui.</em></h2>
      <p>Si vous votez aujourd’hui, pensez à Kaïs &amp; Michel. Deux voix pour la vôtre.</p>
      <a href="#binome" class="button button-light">Découvrir le binôme <span aria-hidden="true">↗</span></a>
    </div>
  </section>

  <footer class="site-footer">
    <div class="container footer-inner">
      <a href="#accueil" class="footer-brand">TG<span>4</span> · KAÏS &amp; MICHEL</a>
      <span>Élection des délégués · 28.09.2026</span>
      <a href="#accueil" class="footer-top">Retour en haut ↑</a>
    </div>
  </footer>
</div>
