<script>
    import { charities } from '../data/charities.js';
    // import { params } from '../stores/pages.js';
    import Header from '../components/Header.svelte';
    import Footer from '../components/Footer.svelte';
    import Loader from '../components/Loader.svelte';

    let name, amount, email, agree = false;
    let charity = {};
    export let params;

    function getCharity(id){
        return charities.find(function(charity) {
            return charity.id == parseInt(id)
        } 
        );
    }

    charity = getCharity(params.id)

    console.log(charity)
   
    async function handleFormSubmit(event){
        const newData = await getCharity(params.id)
        newData.pledged = newData.pledged + parseInt(amount);

        console.log(newData)
        try{

            const resMid = await fetch(`/.netlify/functions/payment`,{
                method: 'POST',
                headers: {
                    'content-type': 'application/json'
                },
                body: JSON.stringify({
                    id: params.id,
                    amount: parseInt(amount),
                    name,
                    email
                })}
            )

            const midtransData = await resMid.json();
            console.log(midtransData);
            window.location.href = midtransData.url;
        }catch(err){
            console.log(err);
        }
           
    }

</script>

<style>
    #xs-input-checkbox{
        display: flex;
        align-items: center;
    }

    #xs-donate-agree{
        width: 35px;
    }

    label[for="xs-donate-agree"]{
        margin: 0;
        margin-left: 10px;
    }

    .xs-donation-form-images{
        text-align: center;
    }
</style>

<Header />

<!-- welcome section -->
	<!--breadcumb start here-->
{#if !charity}
<Loader />
{:else}
<section class="xs-banner-inner-section parallax-window" style=
"background-image:url('/assets/images/backgrounds/kat-yukawa-K0E6E0a0R3A-unsplash.jpg')">
    <div class="xs-black-overlay"></div>
    <div class="container">
        <div class="color-white xs-inner-banner-content">
            <h2>Donate Now</h2>
            <p>{charity.title}</p>
            <ul class="xs-breadcumb">
            <li class="badge badge-pill badge-primary">
                <a href="/" class="color-white">Home /</a> Donate
            </li>
            </ul>
        </div>
    </div>
</section><!--breadcumb end here-->
<!-- End welcome section -->
    
<main class="xs-main">
    <!-- donation form section -->
    <section class="xs-section-padding bg-gray">
        <div class="container">
            <div class="row">
                <div class="col-lg-6">
                    <div class="xs-donation-form-images">
                        <img src="{charity.thumbnail}" class="img-responsive" alt="Family Images">
                    </div>
                </div>
                <div class="col-lg-6">
                    <div class="xs-donation-form-wraper">
                        <div class="xs-heading xs-mb-30">
                            <h2 class="xs-title">{charity.title}</h2>
                            <p class="small">To learn more about make donate charitywith us visit our "<span class="color-green">Contact
                            us</span>" site. By calling <span class="color-green">+44(0) 800 883 8450</span>.</p>
                            <span class="xs-separetor v2"></span>
                        </div><!-- .xs-heading end -->
                        <form action="#" on:submit|preventDefault={handleFormSubmit} method="post" id="xs-donation-form" class="xs-donation-form" name="xs-donation-form">
                            <div class="xs-input-group">
                                <label for="xs-donate-name">Donation Amount <span class="color-light-red">**</span></label> 
                                <input required="true" type="text" name="amount" bind:value={amount} id="xs-donate-name" class="form-control" 
                                placeholder="Your donation in IDR">
                            </div><!-- .xs-input-group END -->
                            <div class="xs-input-group">
                                <label for="xs-donate-charity">Your Name <span class="color-light-red">**</span></label>
                                <input required="true" type="text" name="name" bind:value={name} class="form-control" placeholder="Your awesome name">
                            </div><!-- .xs-input-group END -->
                            <div class="xs-input-group">
                                <label for="xs-donate-charity">Your Email <span class="color-light-red">**</span></label>
                                <input required="true" type="text" bind:value={email} name="email" class="form-control" placeholder="email@awesome.com">
                            </div><!-- .xs-input-group END -->
                            <div class="xs-input-group" id="xs-input-checkbox">
                                <input type="checkbox" name="agree" bind:checked={agree}  class="form-control" id="xs-donate-agree">
                                <label  for="xs-donate-agree">I Agree <span class="color-light-red">**</span></label>
                            </div>
                            <button type="submit" 
                            disabled={!agree} class="btn btn-warning"><span class="badge"><i class="fa fa-heart"></i></span> Donate now</button>
                        </form>
                    <!-- .xs-donation-form #xs-donation-form END -->
                    </div>
                </div>
            </div><!-- .row end -->
        </div><!-- .container end -->
    </section><!-- End donation form section -->
</main>
{/if}

<Footer />