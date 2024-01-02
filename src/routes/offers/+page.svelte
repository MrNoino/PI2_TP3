<svelte:head>
    <title>Food Saver - Ofertas</title>
</svelte:head>

<script>
// @ts-nocheck


    import { goto } from '$app/navigation';
    import { beforeUpdate, onMount } from 'svelte';

    beforeUpdate(() => {

        if (!sessionStorage.getItem("token")){

            goto("/");

        }

    });

    let status;
    let data;
    let error;

    onMount(() => {

        getOffers();

    });

    async function getOffers(){

        try{

            var url = new URL('http://127.0.0.1:5000/api/entities/offers/');
            var response = await fetch(url, {
                method : 'GET',
                mode: 'cors',
                headers: {
                'Accept': 'application/json',
                'Authorization': sessionStorage.getItem("token")
                }
            });

            data = await response.json();

            status = response.status;
            
            if (status != 200){

                error = data && data.message ? data.message : "URL não é valido";

            }


        }catch (e){

            error = "Erro ao conectar à API";

        }

        

    }

</script>


<main class="container mb-5">

    <div class="row justify-content-center mt-3 align-items-center">

        {#if !data && !error}
            <div class="col-12 d-flex justify-content-center align-items-center">

                <div class="spinner-border text-secondary" role="status">
                </div>
                <span class="p-2">A carregar...</span>

            </div>
        {/if}

        {#if status == 200 && data != undefined}

            <ul class="list-group col-12">

                {#each data as offer}

                    <div class="list-group-item d-flex justify-content-between">

                        <a href="/offers/{offer.id}" class="w-100 text-decoration-none text-black">
                            <div class="h-100 d-flex justify-content-between align-items-center">
                            
                                <span>Oferta: {offer.description}</span>
                                <span>Preço: {offer.price + "€"}</span>

                            </div>
                        </a>
                        
                    </div>

                {/each}

            </ul>

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
