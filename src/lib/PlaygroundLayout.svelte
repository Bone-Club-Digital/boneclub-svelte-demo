<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
	const title = 'Bone Club Exterior';
	const href = 'https://svelte.dev/playground/a6f893a4c2ab4600b588e6c442ae5efa?version=5.55.0#H4sIAAAAAAAAE80biY7iyPVXKmw2QATGXN00fWRzTKQoO9IqGyWZpKPZwi6gtn2tbZpmGP49r8rlulwGZpVI0WhoXMd7r979XpljJ8Ex6Sw7v0sTgn4f7Vbo3VtJcprmnUFnTSNSdJb_OnbKQ8ZWsQEYF3t-m2Ve8Uqiko2tcEFc40GalCQpAUznoQhympVPz8lzSeMszUt0RGnyPt0lJTqhdZ7GqFtt7N7DIlgGu4sS7TB6RAl-pRtcprm3K0j-2w0AvVdLaPE9XuOcokc29lyOCv44ol5JirK3w330q19VU78YBVtART7jJMxTGn4GqtLi8_qNfZJwA3-0bYKQiDAc79MVcACIWeOoIPf1BJACLEs2jYnVrizT5I84dE0W23T_bbqhwIAQR43pKN3A3OZPiT5TU5JUMvobDUn6N1rQlYMqY9FfCA4P55e8iwzS_hiluAQCvsMJiQoHEUVJcPwtWZcwd7u4N4b_mmYwemON_p2G5RbGx75vLc9xUkS4JP-A2eG8bfaDmrVpkJJZTL15gxY5e3tjz3Ka1O5ZG2FyyRny7DWGFghOjW81CqqZilmLhT3cxhRrusmVT2kafx9gTsrEmyt8bIJBm0yNIQ5hOtMgREwv_6mBmXq3941JBmq6aI5XFE0MG2Z4_rDLQaVSptGzhdCBajZmJvAt6OhfaczR-cb0KsLBy-_TV5JrICYT3zdQZLWm_ouNPZfH6s9zWVKgb4m630X40B3IUfJWwuA7ZgMooq8ExbgMtqQYoCynr8BZlAPRBQI_gQLmGksMdlagdZojEpENBrcVpHFGAD7bngF4r1uBPw1aaIBD5kmDiO-3OM9Igg7pLkcbcKNoT8FUAFsBRxuAosGxyeYwAGJwdChoRRV5ywh40c2OhjgJyEXkf2cHdHDgx_SA1gSXu5yEiguAFNRbnJ8mBCZogFYgCqAwBhkI31557nJLOJcuEvF9GlAcNah4T0CFYhKvSA64f0wphw8iL7jAtQNTAmfl-HAZp0W2JTlBK3JIYQEbXaU4Dy-TsU2zJivesijNBQggg4t9k6ah4ALOyXoXRQcUMD0EZqm4mVESkEKiZX_-LdRTxLher48en5CgRCj-T6Cve5qE6d7jjH9PQop73V6M34Z75puW6GbmZ2_9bh2KZFTMQqajj8gAC6FVhan4J09I816QJWFUu3t9MQErcRi-ewVxfkvBrSUk73WDLU42pDsQqBQBOQFVSQRi2JqTGIzzit3lqQay3iUBN2RYFUaEWyHAE6ega9STUfXzZyOQ9lGFXlJjRdky3xFxKi0w16N8vCAlczTpzpIJzNhBWYN2GqD3uNwCS996_sD0aEPThfXFaSuEuDgkAbJOzLGYJ5Yhv3FEPRnQKDqTQgDES0lCtf9snDdYVR3D1jY3FoPd1emsZKOvYID55Qf90QL7LvIyDFmf1Fb3GrDJHCQuQohvrMV7TBv5jsc8tgH0BCYO9gLK1zfpYSaXRsQDQfS6fxJg0CuDw_0-2uMCwlQavIBTgJkkLUGGLOc6kNIDIyAGmvrrSWpWM8QZKmQpT00BT-s0JXKlfFJfzoN6l4QkVKCcqmGD-uUS-BWTHPNkBOJiDIukrlaAfoN-KOtUpffLo0oeTl8PkPb84fR1HxUs3dBX8fzj1P-hgrWUqXbNQAv6JwX4kw3zUwMcA9hV233Y58sN4z4rQh5GqmhJHkL6ynYGES6Kx-dOEYCne-7IoWVN3eOx_nZSk4IvQ5o8HiWPtPlaeMPXyooejy7b0jYwEQ2rpOfx2JQXrOSlVk21oruSWUU4SLo8ALL66bkcDhmnhpwLS6Sx7d5a8SZmgeH21EFMfdCmylpHYM7SGgmaE_FUPTC6UU3yavPceXoYwdDTc6Lma9hSIiwhH-5znKnz2OdjyeoaJH-UgfI3jTpiqUa0E8AZ0syxUVUYSzlgbBOBPAaHfzeBYD5owtDrkKU2dHrd655DY6JS3CZBdtmytMeZjbRv-9CyjdmTIkZw9KkeeODuUNFqCEXnv1BfXqGDlovS_aQW4B0wGryqGol3kG1pEkzTTD2xpQVNIshRtcGcgDmEgJ9B09HvyeqFlkNtFyxirk0telKrj19B8HLQCOctIFuHTLTIAwCwLcusWI5G-_3eW0FWyHPhIPV2LyPOgIkHOdJzB7FWymOXs2oUZ7N7yHJDEjA-bV-D8XOni0Y69iVYMvmZaOP0hXhw2rhG-9yp8IoxA9GIriWahxFfVxtibXalboCGeWq-C_YNgbMkBwSVoM_7sxqJoTwryIiX5ZYW1qZ3kaTRjVqJ0KEUbSqRJks2TkLIU_Hj0RFiT9rSACcMdrnN091me3E5YXHVXsWD7cm2n0qwSiYXJbzFr-QjGys-Qrad4wCgf9yTCKpS8vHw8c9w-M3Hqf_x42w-vp17oG66HRgqYcyNpOSlHlTPuszXItqIENQUtyMkybMevyIY8ixRskPuxL8NIEMLyZum7mcwAsLKtR-rFICypGYYEpDNEuI_h4R-jXxvOj8V9z8o3Bzudvp05GA8XhCeHkYwYqzI5AKoDWE-06aVSfDDjNhhTpespZqvD8MzBCNUVexb8zrm8ahXNQK0HclEHFP9JT3a8kglG0zuOKyFELvnxAOE3WnS3L8Rqat1Sk44CQ7DVZlU6Rr7pjxQtRaemSlFNHipbYOXgEpKDwVwX2c5e5Yo4CF_4SmBscx-NPawzkWYQ-BobjPWRXjFtOup6gzJCt_Y8zCqznHeLfKasM0bmsWb9IPN_SGNL-9u5kYWlADn4WUwkiNNACwkM2dq52HOxVtwg-zQugC3k6f3vL-DOEawuIk-nQmG140k3eKuwLhKw4OJj8tRKSUlUWgsqHXsXYxpZGkEn6VJtitrvSVsFTAQfH9AtmkEx2O4-YG-0fyyHVgfRpwOjfgvIO07WLBP8_AidZlY2CCw4invK2prLhLY5C_mRWNhk2mZvpAFmH6W0xjnhysMn2vD6UkohWZXl5EUBArz0Inm6S_kpx1hV0PJKy15Re0Abjpy_emKrGdNE6ADHDZpGpaq8KS5KDgP3JNz4Ms8Tcu66B4OV8EwXa-X6CsyvsGity7Gt6w5ADPT21tyu9ZnmPbBxPpuvSDGBI9fS5RvVrg3ng1Q_d_vN1YBkDxk8PniyexugCazBXxM4ZvvzRZGO8LjpW9NdpZWwRc2Q_RlvXABXtQ9Y9-vw4_W1hzPWF9TjOMiI0E55J0PmLpBI3QnptixIfLvl2hLw5AkYpj3oSEFS8JhkEYp48x4PG5OghZugDu7POp1z6RSRFx7ej9mm26_CaagnwBKwIhpTioGBDziiRW0SCNcjbOE02BgVQE3OYhXsAmKnRpEUhCI8XUzS8buYZpT0K8aoYl3TyPw97z9KmI826KDEMjkHJp5iwIFuxUFbSKfKMl7IPMJSJ4Jfwx6MK558mnIMysQUlMhPNXjQNYJ3Win3qSBlmH1fP45nkjENZ7V5kt59v-gC5JLsj9x8RRZyiubIWFd9WKJEqDRFsHEaT13Ex_MZzrxm6hrrCEtMp4n85Zlq7VuCd1sS2MoXf3IUK1pueT3P5jWBplmOKAlwATNmTdRe1W9X1MQ07chOEpgGHhz4GYR5IQ0QJk65qoyL_LR0oYG7xTZlxmvxAzepr6iNHVbgENj76ZABBfk4gk8ETPqk1iHbxLU8CUm1BrOzxCnUmingjiPXxNhlYM_Wy4zm4JNTkMxxr4OSxJnrBphbn8XJwWLOhnBZW8-YB02cTszXuf92nQ3OGMxRQWbiG6SIQVABeso19AzHLJiC1RjIZe2KoP72PWpKwwFidZOBGNAID4UVVUABiMO6a6o1jj4XsVydkDxn_npvgEC9gJkYDYNW6L5ZKZ2vA2LLQ4hvFYDz6WPWIsSseBcbferMMA3LvryypQLEPkcmd-CyL_R3ScrvkDVopLRuIp2ORwke5O5iOjLXV7ZMFlXSfuhN7lRWzQTremvLdX37ipLVZfBMj7JuXOqLg24RRkaBu0m1--fQYK2U-k5cc4jvw__lI6swReLwDS2RvfC7G-kw4pIyZS6YIQxfWTxltQJgsimXnHek9nlWdKyBmUOmqaSJlZEDqUrqmOFxOtWpMXEJEF1Ty46GulZpi02Lf1pm6gs1z5hSUtDKRRFXtXCaYjfd6xnRYw8QCtZrQm2dTYpINmAaTDHAuDysm0Jt-WfZspJSr82AV8wmciJYJcXTKbiaDUgzZ1x_mY4r9_mUz5MC7u6Hs2VHjWVeCaVWPNrSDoqCFncV_lKuaHYcrgIpLbzbMYhbt6Auqh7VpC7LiR_Kftrxxnj4mWJmHnhfLhhC4Grvf0WAt1AZzSa-7KP93P24ITGoqxZRzRDkOjQBGph2MPui7KiB2k7RD2rYmQMWy5XZM1esBGME28QLVG321D1FlZyu6g-5pJngrETxUWroNRlbPc-h3N28zscz7_uozwt-dCdH5JNM_XnOS8UK5JT_NUnttbklo-ms2rUdKNoeqMD1jgJAqxwo2kh5KG4ukrLrcHMOkZeq4Dj1ixD-WX9lBftQ65ts4-qw9Ugr81_1dl4e_CxgtncmX030jPpZZe8fVITdK174JskfpGPXETSEI-TtRrsVkAGFy8HZq3ffK1mfFH-zVuLdeJstBxU7qyFhUu5s0auXQRdrHlka_xLD9pSw7lzykZRN9NUXWOa76attbTzvbljB2vRX28ytU_TXi6YVS8X3E3USwPXVRVSp3hGZ5cTvigmzFWiY3i-kgBtQAtXJeHP_7uVxNWFhF5HuCNtSzUBR6nfDYKyYHGmslD1g1QYedY2VWrJEpVi2LrUVkTI15dswJf6dU0ddKMYLgxG3DQQOSyoyRmfNRydnJEzV9GreYCrTNgN3bjhquHVuZnvNBk7Z7prZAF28Dcmm1zzFg6Sqns06RR0L1t9jGvqnPu2kytqM81l_2_qRYOkRrV4UwVdE5rbCdz2m9TLRNjExq4DG2yr6pOadwKWs9HEO0Y2ZH5PVwNt3Xbj3MVvmI9N6lX-db7-nfUdQKsrQAG1WVyY59YqBjsoTKyZyx2kW9VBsptTmpOXi86ldemOtwQaFR_LzLYkp6owfGNM4wcSB4ChNq4sl9o9qJVBtRxp3sri5ToNdrKlKZCfBTadO-Iizy2hNKjDYWuQM1VZ3LxeVLyxclSVbQ35SyC2PqrbU6d5fJGa6N7Elpe7-HcEBaOYmM6tsMDZ7JrQxeCaVx5_6ipQJBs8cVN9NlnX6qBr_Z0G2io-rqwGFBh5ze0C0WyhnLO2aw18vLhITPuprtNudX3-pTm8zIFv7pvIzyX1Z-52ru_0KbovXN2YuYv9fjdyMOCqFGbsSmG-idkPh5D-u6E7Vs_KF_rbLmgu3apM2m5V6lsO2cNRFx2n84RVP2iShBnvEjhfBZD3-gLms3WdaaRR8q0Ja7F1AWnsuZs39rjyLz0DY0UN_1AVlb1Zz0F0Nyu3TnzH3i-VE8jkvolD3Ss1o0MbLhetMxES9EzlXDOyFba6wDAToJvLWzP3zomxU-rbCzmscxyzH2qy7qTYWqYKiFbXiH6f2ZZrAadadOdhnu0suvA8jKr3gTqDDnvXtLNkL6Of_g1PmEbsx4GdJf9R1uk_L2tVwK8_AAA';
	let prefersDark = $state(true);
	let isDark = $state(true);

	function switchTheme() {
		const value = isDark ? 'light' : 'dark';

		isDark = value === 'dark';
		localStorage.setItem('sv:theme', isDark === prefersDark ? 'system' : value);
	}

	$effect(() => {
		document.documentElement.classList.remove('light', 'dark');
		prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;

		const theme = localStorage.getItem('sv:theme');

		isDark = !theme
			? prefersDark
			: theme === 'dark' || theme === 'system' && prefersDark;

		document.documentElement.classList.add(isDark ? 'dark' : 'light');
	});
</script>

<svelte:head>
	<title>--from-playground {title}</title>
	<script>{ const theme = localStorage.getItem('sv:theme'); document.documentElement.classList.add( !theme || theme === 'system' ? window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light' : theme ); }</script>
</svelte:head>

<div class="layout">
	<nav class="navbar">
		<div class="nav-left">
			<a href="/" class="svelte-icon"><img src={favicon} alt="Svelte" width="32" height="32" /></a>
			<p class="title">{title}</p>
		</div>

		<div class="nav-right">
			<a
				href={href}
				class="raised"
				target="_blank"
				rel="noopener noreferrer"
			>
				--to-playground 
				<span aria-hidden="true" style="margin-left:0.25em;">↗</span>
			</a>

			<button
				class="raised theme-toggle"
				onclick={switchTheme}
				aria-label="Toggle theme"
			><span class="icon"></span></button>
		</div>
	</nav>

	<main class="content">{@render children()}</main>
</div>

<style>
	:global(body) {
		margin: 0;
	}

	:global(html) {
		margin: 0;
		--bg-1: hsl(0, 0%, 100%);
		--bg-2: hsl(206, 20%, 90%);
		--bg-3: hsl(206, 20%, 80%);
		--navbar-bg: #fff;
		--fg-1: hsl(0, 0%, 13%);
		--fg-2: hsl(0, 0%, 50%);
		--fg-3: hsl(0, 0%, 60%);
		--link: hsl(208, 77%, 47%);
		--border-radius: 4px;
		--font: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans',
			'Helvetica Neue', sans-serif;
		color-scheme: light;
		background: var(--bg-1);
		color: var(--fg-1);
		font-family: var(--font);
		line-height: 1.5;
		height: calc(100vh - 2rem);
		accent-color: var(--link) !important;
		min-height: 100vh;
		background-color: var(--bg-1);
	}

	:global(html.dark) {
		color-scheme: dark;
		--bg-1: hsl(0, 0%, 18%);
		--bg-2: hsl(0, 0%, 30%);
		--bg-3: hsl(0, 0%, 40%);
		--navbar-bg: hsl(220, 14%, 16%);
		--fg-1: hsl(0, 0%, 75%);
		--fg-2: hsl(0, 0%, 40%);
		--fg-3: hsl(0, 0%, 30%);
		--link: hsl(206, 96%, 72%);
	}

	.navbar {
		color: var(--fg-1);
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0em 2.5rem;
		height: 3.7rem;
		background-color: var(--navbar-bg);
		box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.08),
			0 1.5px 4px 0 rgba(0, 0, 0, 0.04);
	}

	.nav-left {
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}

	.svelte-icon {
		display: flex;
		align-items: center;
		text-decoration: none;
		transition: opacity 0.2s ease;
	}

	.svelte-icon:hover {
		opacity: 0.8;
	}

	.title {
		font-size: 1.5rem;
		font-weight: 400;
		margin: 0;
	}

	.nav-right {
		display: flex;
		align-items: center;
		gap: 1rem;
	}

	.raised {
		background: var(--navbar-bg);
		border-left: 0.5px solid var(--fg-3);
		border-top: 0.5px solid var(--fg-3);
		border-bottom: none;
		border-right: none;
		border-radius: var(--border-radius);
		color: var(--fg-1);
		cursor: pointer;
		transition: all 0.2s ease;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1),
			0 1px 2px rgba(0, 0, 0, 0.06);
		text-decoration: none;
		font-weight: 500;
		padding: 0.25rem 0.75rem;
		font-size: 0.8rem;
	}

	.raised:hover {
		border-left-color: var(--fg-2);
		border-top-color: var(--fg-2);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15),
			0 2px 4px rgba(0, 0, 0, 0.1);
		transform: translate(-1px, -1px);
	}

	.content {
		padding: 1rem;
		color: var(--fg-1);
	}

	.theme-toggle {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 1.8rem;
		height: 1.8rem;
		padding: 0;
		min-width: 2rem;
	}

	.icon {
		display: inline-block;
		width: 1.5rem;
		height: 1.5rem;
		-webkit-mask-size: 1.5rem;
		mask-size: 1.5rem;
		-webkit-mask-repeat: no-repeat;
		mask-repeat: no-repeat;
		-webkit-mask-position: center;
		mask-position: center;
		background-color: var(--fg-1);
	}

	.icon {
		mask-image: url('data:image/svg+xml,%3csvg%20xmlns="http://www.w3.org/2000/svg"%20viewBox="0%200%2024%2024"%3e%3cpath%20fill="%23666"%20d="M12%2021q-3.775%200-6.388-2.613T3%2012q0-3.45%202.25-5.988T11%203.05q.625-.075.975.45t-.025%201.1q-.425.65-.638%201.375T11.1%207.5q0%202.25%201.575%203.825T16.5%2012.9q.775%200%201.538-.225t1.362-.625q.525-.35%201.075-.037t.475.987q-.35%203.45-2.937%205.725T12%2021Zm0-2q2.2%200%203.95-1.213t2.55-3.162q-.5.125-1%20.2t-1%20.075q-3.075%200-5.238-2.163T9.1%207.5q0-.5.075-1t.2-1q-1.95.8-3.163%202.55T5%2012q0%202.9%202.05%204.95T12%2019Zm-.25-6.75Z"/%3e%3c/svg%3e');
	}

	:global(html.dark) .icon {
		mask-image: url("data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%2024%2024'%3e%3cpath%20fill='%23d4d4d4'%20d='M12%2019a1%201%200%200%201%20.993.883L13%2020v1a1%201%200%200%201-1.993.117L11%2021v-1a1%201%200%200%201%201-1zm6.313-2.09.094.083.7.7a1%201%200%200%201-1.32%201.497l-.094-.083-.7-.7a1%201%200%200%201%201.218-1.567l.102.07zm-11.306.083a1%201%200%200%201%20.083%201.32l-.083.094-.7.7a1%201%200%200%201-1.497-1.32l.083-.094.7-.7a1%201%200%200%201%201.414%200zM4%2011a1%201%200%200%201%20.117%201.993L4%2013H3a1%201%200%200%201-.117-1.993L3%2011h1zm17%200a1%201%200%200%201%20.117%201.993L21%2013h-1a1%201%200%200%201-.117-1.993L20%2011h1zM6.213%204.81l.094.083.7.7a1%201%200%200%201-1.32%201.497l-.094-.083-.7-.7A1%201%200%200%201%206.11%204.74l.102.07zm12.894.083a1%201%200%200%201%20.083%201.32l-.083.094-.7.7a1%201%200%200%201-1.497-1.32l.083-.094.7-.7a1%201%200%200%201%201.414%200zM12%202a1%201%200%200%201%20.993.883L13%203v1a1%201%200%200%201-1.993.117L11%204V3a1%201%200%200%201%201-1zm0%205a5%205%200%201%201-4.995%205.217L7%2012l.005-.217A5%205%200%200%201%2012%207z'/%3e%3c/svg%3e");
	}
</style>