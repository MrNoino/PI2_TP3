<svelte:head>
    <title>Food Saver - {offer && offer.description ? offer.description : "Oferta"}</title>
</svelte:head>

<script>
// @ts-nocheck


    import { goto } from '$app/navigation';
    import { beforeUpdate, onMount } from 'svelte';
    import { page } from '$app/stores';

    beforeUpdate(() => {

        if (!sessionStorage.getItem("token")){

            goto("/");

        }

    });

    let offer_status;
    let offer;
    let bought_status;
    let bought;
    let error;
    let id = $page.params.id;

    async function getOffer(id){

        try{

            var url = new URL('http://127.0.0.1:5000/api/offers/'+ id +'/');
            var response = await fetch(url, {
                method : 'GET',
                mode: 'cors',
                headers: {
                'Accept': 'application/json',
                'Authorization': sessionStorage.getItem("token")
                }
            });

            offer = await response.json();

            offer_status = response.status;
            
            if (status != 200){

                error = offer && offer.message ? offer.message : "URL não é valido";

            }


        }catch (e){

            error = "Erro ao conectar à API";

        }



    }

    async function buy(){

        try{

            var url = new URL('http://127.0.0.1:5000/api/entities/offers/buy/');
            var response = await fetch(url, {
                method : 'POST',
                mode: 'cors',
                headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json',
                'Authorization': sessionStorage.getItem("token")
                },
                body: JSON.stringify({
                    offer_id: id
                })
            });

            bought = await response.json();

            bought_status = response.status;

            if(bought_status != 200){

                error = bought.message ? bought.message : "Não foi possivel realizar a operação";

            }

        }catch (e){

            error = "Erro ao conectar com a API";

        }

    }

    onMount(() => {

        getOffer(id);

    });

</script>


<main class="container mb-5">

    {#if bought_status}

        <div class="row justify-content-center mt-3 align-items-center">

            <div class="col-12">

                {#if (bought_status != 200)}

                    <div class="alert alert-warning d-flex align-items-center" role="alert">
                        <img src="/assets/warning.svg" alt="Icon de aviso" height="25"/>
                        <div class="ms-1">
                            {error}
                        </div>
                    </div>

                {:else}

                    <div class="alert alert-success d-flex align-items-center" role="alert">
                        <img src="/assets/success.svg" alt="Icon de sucesso" height="25"/>
                        <div class="ms-1">
                            Compra efetuada com sucesso
                        </div>
                    </div>

                {/if}
            
            </div>

        </div>

    {/if}

    <div class="row justify-content-center mt-3 align-items-center">

        {#if offer_status == 200 && offer != undefined}

            <div class="col-12 col-md-8 col-lg-6 shadow p-2 rounded">

                <h1 class="text-center">{offer.name ? offer.name : ""}</h1>

                <div class="d-flex justify-content-center mt-3">

                    <figure class="figure" style="width: inherit;">
                        <!-- svelte-ignore a11y-img-redundant-alt -->
                        <img class="figure-img img-thumbnail " src="{offer.image ? "http://localhost/food-saver/resources/images/offers/" + offer.image : ""}" alt="Image of the offer"/>
                        <figcaption class="figure-caption text-end">{offer.description ? offer.description : ""}</figcaption>
                    </figure>

                </div>

                <div class="m-2 d-flex justify-content-center">

                    <strong>Total:</strong>
                    <p class="ms-1">{offer.price ? offer.price + "€" : ""}</p>

                </div>

                <form class="d-flex justify-content-center mt-3 mb-3" on:submit|preventDefault={buy} method="POST">

                    <input class="btn btn-primary w-75 shadow" name="buy" type="submit" value="Comprar"/>

                </form>

            </div>


            
        {:else if error}

            <div class="col-12">
                
                <div class="alert alert-warning d-flex align-items-center" role="alert">
                    <img src="/assets/warning.svg" alt="Icon de aviso" height="25"/>
                    <div class="ms-1">
                        {error}
                    </div>
                </div>
            
            </div>

        {/if}

    </div>

</main>