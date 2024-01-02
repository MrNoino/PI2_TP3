<svelte:head>
    <title>Food Saver - Compras</title>
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

    onMount(() => {

        getPurchases();

    });

    async function getPurchases(){

        var url = new URL('http://127.0.0.1:5000/api/users/purchases/');
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

    }

</script>


<main class="container mb-5">

    <div class="row justify-content-center mt-3 align-items-center">

        {#if !data}
            <div class="col-12 d-flex justify-content-center align-items-center">

                <div class="spinner-border text-secondary" role="status">
                </div>
                <span class="p-2">A carregar...</span>

            </div>
        {/if}

        {#if status == 200 && data != undefined}

            <ul class="list-group col-12">

                {#each data as purchase}

                    <li class="list-group-item d-flex justify-content-between">
                        
                            <span>Oferta: {purchase.offer.description}</span>
                            <span>Data: {new Date(purchase.offer.date).toLocaleDateString()}</span>
                            <span>Preço: {purchase.offer.price + " €"}</span>
                        
                    </li>

                {/each}

            </ul>

        {:else if status == 404}
            <div class="col-12">
            
                <div class="alert alert-warning d-flex align-items-center" role="alert">
                    <img src="/assets/warning.svg" alt="Icon de aviso" height="25"/>
                    <div class="ms-1">
                        {data.message ? data.message : "URL não é valido"}
                    </div>
                </div>
            
            </div>
        {/if}

    </div>

</main>