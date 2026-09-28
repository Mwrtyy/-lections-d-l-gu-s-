<script>
  import { onMount } from "svelte";
  import gsap from "gsap";
  import ScrollToPlugin from "gsap/ScrollToPlugin";
  import { getLenis } from "$lib/scroll.js";

  let header = $state();
  let brand = $state();
  let navRefs = $state([]);
  let menu = $state();
  let menuLinks = $state([]);
  let isMenuOpen = $state(false);
  const links = [
    { label: "Notre idée", href: "#manifeste" },
    { label: "Les engagements", href: "#engagements" },
    { label: "Le binôme", href: "#binome" }
  ];

  function closeMenu() {
    if (!isMenuOpen) return;
    isMenuOpen = false;
    if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) {
      gsap.set(menu, { autoAlpha: 0, clipPath: "inset(0% 100% 0% 0%)" });
      return;
    }
    gsap.to(menu, { autoAlpha: 0, clipPath: "inset(0% 100% 0% 0%)", duration: 0.42, ease: "power2.inOut" });
  }

  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
    if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) {
      gsap.set(menu, { autoAlpha: isMenuOpen ? 1 : 0, clipPath: isMenuOpen ? "inset(0% 0% 0% 0%)" : "inset(0% 100% 0% 0%)" });
      return;
    }
    if (isMenuOpen) {
      gsap.timeline()
        .to(menu, { autoAlpha: 1, clipPath: "inset(0% 0% 0% 0%)", duration: 0.62, ease: "power2.inOut" })
        .fromTo(menuLinks, { y: 18, autoAlpha: 0 }, { y: 0, autoAlpha: 1, duration: 0.62, stagger: 0.09, ease: "power2.out" }, "-=0.32");
    } else {
      gsap.to(menu, { autoAlpha: 0, clipPath: "inset(0% 100% 0% 0%)", duration: 0.42, ease: "power2.inOut" });
    }
  }

  function scrollTo(event, href) {
    event.preventDefault();
    closeMenu();
    const target = document.querySelector(href);
    if (!target) return;
    history.replaceState(null, "", href);
    const reduced = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    const lenis = getLenis();
    if (lenis && !reduced) lenis.scrollTo(target, { offset: -82, duration: 1.05 });
    else if (!reduced) gsap.to(window, { duration: 0.95, scrollTo: { y: target, offsetY: 82 }, ease: "expo.inOut" });
    else target.scrollIntoView({ block: "start", behavior: "auto" });
  }

  onMount(() => {
    gsap.registerPlugin(ScrollToPlugin);
    if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) return;
    const intro = gsap.timeline({ delay: 0.16 });
    gsap.set(header, { y: -18, autoAlpha: 0 });
    gsap.set([brand, navRefs], { y: 10, autoAlpha: 0 });
    intro.to(header, { y: 0, autoAlpha: 1, duration: 0.75, ease: "expo.out" })
      .to(brand, { y: 0, autoAlpha: 1, duration: 0.5, ease: "power3.out" }, "-=0.35")
      .to(navRefs, { y: 0, autoAlpha: 1, duration: 0.45, stagger: 0.07, ease: "power3.out" }, "-=0.3");
    const escape = (event) => { if (event.key === "Escape") closeMenu(); };
    window.addEventListener("keydown", escape);
    return () => { intro.kill(); window.removeEventListener("keydown", escape); };
  });
</script>

<header bind:this={header} class="header">
  <a href="#accueil" onclick={(e) => scrollTo(e, "#accueil")} bind:this={brand} class="brand" aria-label="Retour en haut, campagne TG4">
    <span class="brand-mark">TG<span>4</span></span><span class="brand-divider"></span><span class="brand-caption">DÉLÉGUÉS<br />2026</span>
  </a>
  <nav class="desktop-nav" aria-label="Navigation principale">
    {#each links as link, i}<a bind:this={navRefs[i]} href={link.href} onclick={(e) => scrollTo(e, link.href)}>{link.label}</a>{/each}
  </nav>
  <a class="header-cta" href="#vote" onclick={(e) => scrollTo(e, "#vote")}>Le vote, aujourd’hui <span aria-hidden="true">↘</span></a>
  <button class="menu-button" aria-label={isMenuOpen ? "Fermer le menu" : "Ouvrir le menu"} aria-controls="mobile-menu" aria-expanded={isMenuOpen} onclick={toggleMenu}>
    <span class:close-icon={isMenuOpen} class="menu-icon" aria-hidden="true"><i></i><i></i></span>
  </button>
</header>

<div id="mobile-menu" bind:this={menu} class="mobile-menu" aria-hidden={!isMenuOpen}>
  <nav aria-label="Navigation mobile">
    {#each links as link, i}
      <a bind:this={menuLinks[i]} href={link.href} tabindex={isMenuOpen ? 0 : -1} onclick={(e) => scrollTo(e, link.href)}>
        <span class="mobile-number">0{i + 1}</span>{link.label}<span aria-hidden="true">↗</span>
      </a>
    {/each}
    <a bind:this={menuLinks[3]} class="mobile-vote" href="#vote" tabindex={isMenuOpen ? 0 : -1} onclick={(e) => scrollTo(e, "#vote")}>Le vote, c’est aujourd’hui <span aria-hidden="true">↘</span></a>
  </nav>
  <p class="mobile-menu-note">Kaïs &amp; Michel · Terminale générale 4</p>
</div>

<style>
  .header { position: fixed; z-index: 60; top: 16px; left: 50%; transform: translateX(-50%); width: min(calc(100% - 32px), 1160px); min-height: 64px; padding: 0 9px 0 18px; display: flex; align-items: center; justify-content: space-between; gap: 1rem; border: 1px solid rgba(23,23,21,.1); border-radius: 999px; background: rgba(251,250,246,.86); box-shadow: 0 8px 28px rgba(25,24,21,.055); backdrop-filter: blur(18px); }
  .brand { display: inline-flex; align-items: center; gap: 11px; flex-shrink: 0; text-decoration: none; }
  .brand-mark { font-size: 1.25rem; font-weight: 700; letter-spacing: -.09em; }.brand-mark span { color: var(--accent); }
  .brand-divider { height: 24px; width: 1px; background: var(--line); }
  .brand-caption { font-size: .53rem; line-height: 1.15; letter-spacing: .12em; color: var(--muted); font-weight: 600; }
  .desktop-nav { display: flex; align-items: center; gap: clamp(1rem, 2.2vw, 2.3rem); }
  .desktop-nav a { text-decoration: none; color: var(--ink-soft); font-size: .78rem; font-weight: 500; transition: color .2s ease; }.desktop-nav a:hover { color: var(--accent); }
  .header-cta { min-height: 46px; padding: 0 18px; display: inline-flex; align-items: center; gap: 12px; border-radius: 999px; background: var(--ink); color: var(--paper-light); font-size: .78rem; font-weight: 600; text-decoration: none; transition: background .25s ease, transform .25s ease; }
  .header-cta:hover { background: var(--accent); transform: translateY(-1px); }
  .menu-button { display: none; height: 46px; width: 46px; border: 0; border-radius: 50%; background: var(--ink); color: white; cursor: pointer; }
  .menu-icon { display: flex; flex-direction: column; gap: 5px; width: 17px; margin: auto; }.menu-icon i { display: block; height: 1.5px; width: 100%; background: currentColor; transition: transform .25s ease; }
  .close-icon i:first-child { transform: translateY(3.25px) rotate(45deg); }.close-icon i:last-child { transform: translateY(-3.25px) rotate(-45deg); }
  .mobile-menu { position: fixed; z-index: 55; inset: 0; padding: 110px 28px 36px; display: flex; flex-direction: column; justify-content: space-between; background: var(--paper); visibility: hidden; opacity: 0; clip-path: inset(0 100% 0 0); }
  .mobile-menu nav { display: flex; flex-direction: column; }.mobile-menu nav a { display: grid; grid-template-columns: 36px 1fr auto; align-items: center; padding: 19px 0; border-bottom: 1px solid var(--line); color: var(--ink); font-family: var(--font-serif); font-size: clamp(2rem, 9vw, 3rem); line-height: 1; text-decoration: none; }
  .mobile-menu nav a > span:last-child { font-family: var(--font-sans); font-size: .8rem; color: var(--accent); }.mobile-number { font-family: var(--font-sans); font-size: .68rem; letter-spacing: .1em; color: var(--muted); }
  .mobile-menu nav .mobile-vote { margin-top: 22px; padding: 18px 20px; border: 0; border-radius: 16px; background: var(--accent); color: white; font-family: var(--font-sans); font-size: 1rem; font-weight: 600; }.mobile-menu nav .mobile-vote > span:last-child { color: white; }
  .mobile-menu-note { margin: 0; font-size: .7rem; letter-spacing: .12em; text-transform: uppercase; color: var(--muted); }
  @media (max-width: 820px) { .header { padding-left: 15px; }.desktop-nav, .header-cta { display: none; }.menu-button { display: block; } }
</style>
