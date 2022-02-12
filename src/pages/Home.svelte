<script>
	import CharityList from '../components/CharityList.svelte';
	import Header from '../components/Header.svelte';
	import Welcome from '../components/Welcome.svelte';
	import Promo from '../components/Promo.svelte';
	import Footer from '../components/Footer.svelte';

	import { charities } from '../data/charities';
	
	let title = "Charity";
	setTimeout(() => {
		title = "Donasi";
	}, 2000);

</script>


<style>
	

</style>

<div>
	<Header/>
	<Welcome/>
	<CharityList {charities}/>
	<Promo />
	<Footer />
</div>