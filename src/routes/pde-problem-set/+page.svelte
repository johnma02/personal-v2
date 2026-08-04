<script lang="ts">
	import { onMount } from 'svelte';

	const problems = [
		['problem-1', 'Derive the point-source potential from conservation of flux'],
		['problem-2', 'The Green’s function as a bent string'],
		['problem-3', 'Fourier transforms and Green’s functions are the same inverse seen two ways'],
		['problem-4', 'Reciprocity without calculating any Green’s function'],
		['problem-5', 'A larger domain has a larger Green’s function'],
		['problem-6', 'Why the Neumann Green’s function must be different'],
		['problem-7', 'Discover the method of images'],
		['problem-8', 'Characterize the Gaussian from PDE principles'],
		['problem-9', 'Infinite propagation for heat, finite propagation for waves'],
		['problem-10', 'Exactly when is a Green’s function positive?']
	];

	onMount(() => {
		const win = window as Window & { MathJax?: unknown };
		win.MathJax = {
			tex: { inlineMath: [['\\(', '\\)']], displayMath: [['\\[', '\\]']], processEscapes: true },
			svg: { fontCache: 'global' }
		};
		const script = document.createElement('script');
		script.src = 'https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js';
		script.async = true;
		document.head.appendChild(script);
	});
</script>

<svelte:head>
	<title>Insight-Driven Undergraduate PDE Problems</title>
	<meta name="description" content="A computation-light undergraduate PDE problem set centered on Green's functions, Fourier transforms, maximum principles, energy methods, scaling, and clever transformations." />
</svelte:head>

<div class="shell">
	<aside>
		<a class="brand" href="#top">Insight-Driven PDE Problems</a>
		<p class="label">START HERE</p>
		<a href="#orientation">Orientation</a>
		<a href="#greens-functions">Green’s functions</a>
		<a href="#maximum-principle">Maximum principle</a>
		<p class="label">PROBLEMS</p>
		{#each problems as problem, i}
			<a href={'#' + problem[0]}><span>{i + 1}</span>{problem[1]}</a>
		{/each}
		<p class="label">FINISH</p>
		<a href="#optional">Optional problems</a>
		<a href="#route">Suggested route</a>
	</aside>

	<main id="top">
		<nav class="actions">
			<a href="/">← Home</a>
			<button onclick={() => window.print()}>Print / save as PDF</button>
		</nav>

		<header>
			<h1>Insight-Driven Undergraduate PDE Problems</h1>
			<p>Green’s functions, Fourier transforms, maximum principles, energy methods, scaling, and clever transformations</p>
			<div class="goal"><strong>Design goal.</strong> These problems use undergraduate PDE tools, but their difficulty comes from finding the right idea rather than carrying out long computations.</div>
		</header>

		<section id="orientation">
			<h2>Orientation</h2>
			<p>The set is a mixture of olympiad-style challenges and theorem-style undergraduate PDE problems. You may use multivariable calculus, ODEs, integration by parts, basic Fourier transforms, elementary eigenfunction ideas, delta functions, standard facts about harmonic, heat, and wave equations, and the simplest form of the maximum principle.</p>
			<p>You should not need Sobolev spaces, distribution theory beyond point sources, functional analysis, or lengthy separation-of-variables calculations.</p>
		</section>

		<section id="greens-functions">
			<h2>Green’s functions in one sentence</h2>
			<p>For a linear boundary-value problem \(Lu=f\), the Green’s function \(G(x,y)\) is the response observed at \(x\) from a unit point source placed at \(y\):</p>
			\[L_xG(x,y)=\delta_y(x).\]
			<p>With the appropriate boundary conditions,</p>
			\[u(x)=\int_\Omega G(x,y)f(y)\,dy.\]
			<p>Thus \(G\) is the kernel of the inverse operator \(L^{-1}\). It packages the operator, geometry, and boundary conditions into one object.</p>
		</section>

		<section id="maximum-principle">
			<h2>A maximum-principle reminder</h2>
			<p>If \(u\) is harmonic in a bounded domain, then its maximum and minimum occur on the boundary unless \(u\) is constant. For the heat equation, the analogous statement uses the parabolic boundary: the initial time together with the spatial boundary.</p>
		</section>

		<article id="problem-1">
			<div class="eyebrow">Problem 1</div><h2>Derive the point-source potential from conservation of flux</h2>
			<p>Let \(\Phi\) be radial away from the origin and satisfy</p>
			\[-\Delta\Phi=\delta_0\qquad\text{in }\mathbb R^d.\]
			<p>Do not begin by mechanically solving the radial Laplace equation. Integrate over a ball \(B_r(0)\) and prove</p>
			\[-\int_{\partial B_r}\frac{\partial\Phi}{\partial\nu}\,dS=1.\]
			<p>Use this conservation law to derive, up to an additive constant,</p>
			\[\Phi(x)=\begin{cases}-|x|/2,&d=1,\\-\frac1{2\pi}\log|x|,&d=2,\\\frac1{(d-2)|S^{d-1}|}|x|^{2-d},&d\ge3.\end{cases}\]
			<p>Then explain why dimension two is logarithmic, why decay at infinity is possible for \(d\ge3\) but not \(d=1,2\), and why the point-source potential is singular at the source in \(d\ge2\) but not in \(d=1\).</p>
			<div class="target"><strong>Conceptual target.</strong> The singularity is dictated by how a fixed amount of flux spreads across spheres.</div>
		</article>

		<article id="problem-2">
			<div class="eyebrow">Problem 2</div><h2>The Green’s function as a bent string</h2>
			<p>Consider</p>
			\[-u''(x)=f(x),\qquad 0<x<1,\qquad u(0)=u(1)=0.\]
			<p>For fixed \(y\in(0,1)\), construct \(G(x,y)\) using only these facts: it is linear in \(x\) on each side of \(y\), continuous at \(y\), zero at both endpoints, and its derivative jumps by</p>
			\[G_x(y^+,y)-G_x(y^-,y)=-1.\]
			<p>Derive</p>
			\[G(x,y)=\begin{cases}x(1-y),&x\le y,\\y(1-x),&x\ge y.\end{cases}\]
			<p>Let \(f\ge0\) have total mass one. For a fixed observation point \(x_0\), where should the source be concentrated to maximize \(u(x_0)\)? Prove</p>
			\[u(x_0)\le x_0(1-x_0).\]
			<div class="target"><strong>Conceptual target.</strong> A Green’s function is the shape of a pinned string bent by a unit point force.</div>
		</article>

		<article id="problem-3">
			<div class="eyebrow">Problem 3</div><h2>Fourier transforms and Green’s functions are the same inverse seen two ways</h2>
			<p>On \(\mathbb R\), solve</p>
			\[u-u''=f,\qquad u(x)\to0\text{ as }|x|\to\infty.\]
			<p>Use the Fourier transform to show</p>
			\[\widehat u(\xi)=\frac{\widehat f(\xi)}{1+\xi^2},\qquad \widehat G(\xi)=\frac1{1+\xi^2}.\]
			<p>Do not use contour integration. In physical space, use that \(G-G''=0\) away from zero, \(G\) decays at both infinities, \(G\) is continuous at zero, and integrating across zero gives a derivative jump. Derive</p>
			\[G(x)=\frac12e^{-|x|}.\]
			<p>Conclude that \(u=G*f\), and explain why multiplication by \(1/(1+\xi^2)\) suppresses high-frequency oscillations.</p>
			<div class="target"><strong>Conceptual target.</strong> Fourier multipliers and Green’s kernels are frequency-space and physical-space descriptions of the same inverse operator.</div>
		</article>

		<article id="problem-4">
			<div class="eyebrow">Problem 4</div><h2>Reciprocity without calculating any Green’s function</h2>
			<p>Let \(u\) and \(v\) solve</p>
			\[-\Delta u=f,\qquad -\Delta v=g\]
			<p>in a bounded domain \(\Omega\), with \(u=v=0\) on \(\partial\Omega\). Prove</p>
			\[\int_\Omega f(x)v(x)\,dx=\int_\Omega g(x)u(x)\,dx.\]
			<p>Write both solutions using the Green’s function and use the arbitrariness of \(f\) and \(g\) to deduce</p>
			\[G(x,y)=G(y,x).\]
			<p>Interpret this as reciprocity: the response at \(x\) to a source at \(y\) equals the response at \(y\) to the same source placed at \(x\).</p>
			<div class="target"><strong>Conceptual target.</strong> Green’s-function symmetry comes from self-adjointness, not from visual symmetry of the domain.</div>
		</article>

		<article id="problem-5">
			<div class="eyebrow">Problem 5</div><h2>A larger domain has a larger Green’s function</h2>
			<p>Let \(\Omega_1\subseteq\Omega_2\) be bounded domains, and let \(G_1,G_2\) be their Dirichlet Green’s functions for \(-\Delta\). Without explicit formulas, prove that for \(x,y\in\Omega_1\),</p>
			\[G_1(x,y)\le G_2(x,y).\]
			<p>The singularities at \(x=y\) are identical. Explain why \(G_2(\cdot,y)-G_1(\cdot,y)\) extends harmonically through \(y\), then use the maximum principle on \(\Omega_1\).</p>
			<p>Deduce that moving a grounded boundary farther from a nonnegative heat source raises the temperature, while drilling a new grounded hole can only lower it.</p>
			<div class="target"><strong>Conceptual target.</strong> A closer absorbing boundary gives heat more opportunity to escape.</div>
		</article>

		<article id="problem-6">
			<div class="eyebrow">Problem 6</div><h2>Why the Neumann Green’s function must be different</h2>
			<p>Suppose one tries to construct</p>
			\[-\Delta_xN(x,y)=\delta_y(x),\qquad \frac{\partial N}{\partial\nu}=0\text{ on }\partial\Omega.\]
			<p>Integrate over \(\Omega\) and show that this is impossible. Find the corrected equation</p>
			\[-\Delta_xN(x,y)=\delta_y(x)-\frac1{|\Omega|}.\]
			<p>Explain why the constant term is forced, why \(N\) is determined only up to an additive constant, why imposing \(\int_\Omega N(x,y)\,dx=0\) restores uniqueness, and why the normalized Neumann Green’s function must take both signs.</p>
			<div class="target"><strong>Conceptual target.</strong> The boundary condition is part of the inverse operator; a perfectly insulating boundary cannot accommodate net source mass.</div>
		</article>

		<article id="problem-7">
			<div class="eyebrow">Problem 7</div><h2>Discover the method of images</h2>
			<p>Let \(H=\{x_d>0\}\), and let \(y^*\) be the reflection of \(y\) across \(x_d=0\). Starting from the whole-space fundamental solution \(\Phi\), show that</p>
			\[G_D(x,y)=\Phi(x-y)-\Phi(x-y^*)\]
			<p>is the Dirichlet Green’s function in \(H\). Explain why the reflected source has opposite sign. Then show that</p>
			\[G_N(x,y)=\Phi(x-y)+\Phi(x-y^*)\]
			<p>has zero normal derivative at the boundary and explain the same-sign image.</p>
			<p>Repeat the reasoning for the heat equation on \(x>0\):</p>
			\[K_D(x,y,t)=K(x-y,t)-K(x+y,t),\qquad K_N(x,y,t)=K(x-y,t)+K(x+y,t).\]
			<p>Determine which kernel preserves total heat and which loses it, interpreting the boundaries as reflecting and absorbing.</p>
			<div class="target"><strong>Conceptual target.</strong> A boundary condition can sometimes be enforced by a carefully signed fictitious source.</div>
		</article>

		<article id="problem-8">
			<div class="eyebrow">Problem 8</div><h2>Characterize the Gaussian from PDE principles</h2>
			<p>Suppose \(\{K_t\}_{t>0}\) is a family of even, nonnegative probability densities on \(\mathbb R\) satisfying</p>
			\[K_s*K_t=K_{s+t},\qquad K_t(x)=\frac1{\sqrt t}K_1\!\left(\frac{x}{\sqrt t}\right).\]
			<p>Assume continuity in \(t\), a finite nonzero second moment, and enough regularity for Fourier transforms. Prove that \(K_t\) must be Gaussian.</p>
			<p>Define \(\varphi_t(\xi)=\widehat K_t(\xi)\). Show</p>
			\[\varphi_{s+t}(\xi)=\varphi_s(\xi)\varphi_t(\xi),\qquad \varphi_t(\xi)=\varphi_1(\sqrt t\,\xi).\]
			<p>Use \(\varphi_t(\xi)=\varphi_{t/2}(\xi)^2\ge0\) to justify a logarithm. Deduce</p>
			\[\varphi_t(\xi)=e^{-\kappa t\xi^2},\qquad K_t(x)=\frac1{\sqrt{4\pi\kappa t}}e^{-x^2/(4\kappa t)}.\]
			<div class="target"><strong>Conceptual target.</strong> Translation invariance, semigroup evolution, mass preservation, and parabolic scaling force the Gaussian.</div>
		</article>

		<article id="problem-9">
			<div class="eyebrow">Problem 9</div><h2>Infinite propagation for heat, finite propagation for waves</h2>
			<h3>Part A: Heat</h3>
			<p>Let \(f\ge0\) be compactly supported and not identically zero. For</p>
			\[u_t=u_{xx},\qquad u(x,0)=f(x),\]
			<p>use the heat kernel to prove \(u(x,t)>0\) for every \(x\in\mathbb R\) and every \(t>0\).</p>
			<h3>Part B: Waves</h3>
			<p>Let \(u_{tt}-u_{xx}=0\). Suppose \(u(x,0)\) and \(u_t(x,0)\) vanish on \([x_0-t_0,x_0+t_0]\). Prove, without d’Alembert’s formula, that \(u(x_0,t_0)=0\).</p>
			<p>Use the energy in the shrinking interval</p>
			\[E(t)=\frac12\int_{x_0-t_0+t}^{x_0+t_0-t}(u_t^2+u_x^2)\,dx.\]
			<p>Differentiate carefully and show that \(E'(t)\) is the negative of a sum of squares at the moving endpoints.</p>
			<div class="target"><strong>Conceptual target.</strong> The heat kernel is positive everywhere, while wave energy cannot outrun the characteristic speed.</div>
		</article>

		<article id="problem-10">
			<div class="eyebrow">Problem 10</div><h2>Exactly when is a Green’s function positive?</h2>
			<p>Let \(\lambda_1\) and \(\phi_1>0\) be the first Dirichlet eigenpair on a bounded domain:</p>
			\[-\Delta\phi_1=\lambda_1\phi_1,\qquad \phi_1=0\text{ on }\partial\Omega.\]
			<p>Consider \(L_\mu=-\Delta-\mu\).</p>
			<h3>Below the first eigenvalue</h3>
			<p>Suppose \(\mu<\lambda_1\) and \(L_\mu u=f\ge0\), with \(u=0\) on the boundary. Using \(u_-=\max\{-u,0\}\) and</p>
			\[\lambda_1=\inf_{v\ne0}\frac{\int_\Omega|\nabla v|^2}{\int_\Omega v^2},\]
			<p>prove \(u\ge0\).</p>
			<h3>At and above the first eigenvalue</h3>
			<p>Explain why \(L_{\lambda_1}\) is not invertible. For \(\mu>\lambda_1\), assuming \(\mu\) is not another eigenvalue, take \(f=\phi_1\) and solve explicitly for \(u\). Show that positive forcing produces a negative solution.</p>
			<p>Conclude, whenever the resolvent exists,</p>
			\[G_\mu(x,y)\ge0\text{ for all }x,y\quad\Longleftrightarrow\quad\mu<\lambda_1.\]
			<div class="target"><strong>Conceptual target.</strong> Positivity of the inverse survives exactly until the spectral parameter reaches the first eigenvalue.</div>
		</article>

		<section id="optional">
			<h2>Optional wildcard problems</h2>
			<h3>Backward heat is unique but unstable</h3>
			<p>On \((0,\pi)\), consider \(u_n(x,t)=e^{-n^2t}\sin(nx)\). Explain why reconstructing initial data from time \(T>0\) multiplies the \(n\)-th Fourier coefficient by \(e^{n^2T}\). Distinguish nonuniqueness from instability, and construct final-time data differing by at most \(\varepsilon\) whose reconstructed initial data differ by at least \(M\).</p>
			<h3>Find the hidden heat equation</h3>
			<p>For viscous Burgers’ equation</p>
			\[u_t+uu_x=\nu u_{xx},\qquad \nu>0,\]
			<p>verify the Cole–Hopf substitution</p>
			\[u=-2\nu\frac{\phi_x}{\phi}=-2\nu\partial_x\log\phi.\]
			<p>Show that Burgers’ equation reduces to \(\phi_t=\nu\phi_{xx}\), up to a time-dependent multiplicative factor that can be absorbed into \(\phi\).</p>
		</section>

		<section id="route" class="route">
			<h2>Suggested route through the set</h2>
			\[1\longrightarrow2\longrightarrow3\longrightarrow4\longrightarrow5\longrightarrow6\longrightarrow7\longrightarrow10.\]
			<p>Problems 8, 9, and 10 are the main boss problems. Problems 1–3 provide the concrete intuition that makes the later arguments feel motivated rather than arbitrary.</p>
		</section>
	</main>
</div>

<style>
	:global(*) { box-sizing: border-box; }
	:global(html) { scroll-behavior: smooth; background: #ece8df; }
	:global(body) { margin: 0; background: radial-gradient(circle at 8% 2%, rgba(255,255,255,.85), transparent 28rem), #ece8df; color: #18212b; font-family: Georgia, 'Times New Roman', serif; font-size: 18px; line-height: 1.72; }
	.shell { width: min(1440px, 100%); margin: 0 auto; display: grid; grid-template-columns: 292px minmax(0, 860px); gap: 34px; align-items: start; justify-content: center; padding: 30px 34px 60px; }
	aside { position: sticky; top: 20px; max-height: calc(100vh - 40px); overflow: auto; padding: 22px 18px; border: 1px solid rgba(23,63,103,.14); border-radius: 18px; background: rgba(255,253,248,.88); box-shadow: 0 10px 30px rgba(31,38,45,.055); font: 13px/1.35 ui-sans-serif, system-ui, sans-serif; }
	aside a { display: block; padding: 5px 7px; border-radius: 8px; color: #44505d; text-decoration: none; }
	aside a:hover { background: #e8f0f7; color: #173f67; }
	aside a span { display: inline-block; width: 24px; color: #83909c; }
	aside .brand { margin-bottom: 16px; color: #18212b; font-size: 15px; font-weight: 800; }
	.label { margin: 17px 7px 7px; color: #697582; font-size: 11px; font-weight: 700; letter-spacing: .12em; }
	main { min-width: 0; overflow: hidden; padding: 54px 72px 72px; border: 1px solid rgba(31,38,45,.1); border-radius: 22px; background: #fffdf8; box-shadow: 0 18px 50px rgba(31,38,45,.09); }
	.actions { display: flex; gap: 10px; margin-bottom: 42px; font-family: ui-sans-serif, system-ui, sans-serif; }
	.actions a, .actions button { min-height: 40px; padding: 8px 14px; border: 1px solid #bac7d2; border-radius: 999px; background: white; color: #173f67; font: 700 13px ui-sans-serif, system-ui, sans-serif; text-decoration: none; cursor: pointer; }
	header { margin-bottom: 54px; padding: 54px 46px 42px; border-radius: 18px; color: white; background: linear-gradient(135deg, #092742, #205784); box-shadow: 0 18px 36px rgba(23,63,103,.18); }
	header h1 { margin: 0; color: white; font-size: clamp(34px, 5vw, 54px); line-height: 1.04; letter-spacing: -.045em; }
	header > p { margin: 22px 0 0; color: #dbe9f5; font-size: 20px; line-height: 1.45; }
	.goal { margin-top: 34px; padding: 22px 24px; border: 1px solid rgba(255,255,255,.2); border-radius: 14px; background: rgba(255,255,255,.08); }
	section, article { scroll-margin-top: 28px; }
	h1, h2, h3 { color: #173f67; font-family: ui-sans-serif, system-ui, sans-serif; line-height: 1.2; letter-spacing: -.025em; }
	section > h2 { margin: 64px 0 18px; font-size: 32px; }
	article { margin-top: 76px; padding-top: 4px; }
	article h2 { margin: 5px 0 20px; padding-bottom: 16px; border-bottom: 2px solid #173f67; font-size: 28px; }
	h3 { margin: 32px 0 10px; font-size: 20px; }
	p { margin: 0 0 1.08em; }
	.eyebrow { color: #245d8d; font: 800 12px ui-sans-serif, system-ui, sans-serif; letter-spacing: .14em; text-transform: uppercase; }
	.target { margin: 28px 0 38px; padding: 19px 22px; border: 1px solid #e2d6ad; border-radius: 14px; background: #f6efd9; }
	.route { margin-top: 54px; padding: 24px; border-radius: 15px; background: #e8f0f7; }
	.route > h2 { margin-top: 0; }
	:global(mjx-container[display='true']) { max-width: 100%; overflow-x: auto; overflow-y: hidden; padding: 4px 0 9px; }
	@media (max-width: 1040px) { .shell { grid-template-columns: minmax(0, 860px); padding: 18px; } aside { display: none; } }
	@media (max-width: 680px) { :global(body) { font-size: 16px; } .shell { padding: 0; } main { padding: 24px 21px 50px; border: 0; border-radius: 0; } header { padding: 37px 25px 30px; } header > p { font-size: 17px; } article h2 { font-size: 24px; } }
	@media print { :global(html), :global(body) { background: white; } .shell { display: block; padding: 0; } aside, .actions { display: none; } main { padding: 0; border: 0; box-shadow: none; } header { color: #18212b; background: none; box-shadow: none; border: 2px solid #173f67; break-after: page; } header h1 { color: #173f67; } header > p { color: #5d6875; } article h2 { break-after: avoid; } .target { break-inside: avoid; } }
</style>
