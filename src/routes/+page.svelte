<svelte:head>
    <title>Food Saver - Iniciar Sessão</title>
</svelte:head>

<script>
// @ts-nocheck

    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

    onMount(() => {

        if (sessionStorage.getItem("token")){

            goto("/offers");

        }

    });

    let email = "";
    let password = "";
    let error = undefined;

    let show_password = false;
    let type = show_password ? "text" : "password";
    let password_img_button = show_password ? "/assets/eye-close.svg" : "/assets/eye.svg";

    function togglePassword(){

        show_password = !show_password;
        type = show_password ? "text" : "password";
        password_img_button = show_password ? "/assets/eye-close.svg" : "/assets/eye.svg";

    }

    async function login(){

        try{

            var url = new URL('http://127.0.0.1:5000/api/users/');
            var response = await fetch(url, {
                method : 'PATCH',
                mode: 'cors',
                headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    email: email,
                    password: password
                })
            });

            var data = await response.json();

            if(response.status == 200){

                error = undefined;
                sessionStorage.setItem("token", data.token);
                goto("/offers");

            }else{
        
                error = data && data.message ? data.message : "Erro no Servidor";

            }


        }catch (e){

            error = "Erro ao conectar com a API";

        }

    }

</script>

<main class="container mb-5">

    <div class="row justify-content-center mt-3">

        <div class="col-12 col-md-8 col-lg-6 text-center">

            <img src="/assets/logo.jpeg" class="rounded mx-auto size-inherit" alt="Logo">

        </div>

    </div>

    <div class="row justify-content-center mt-3">

        <div class="col-12 col-md-8 col-lg-6 text-center">

            <h2 class="text-primary">Iniciar Sessão</h2>

        </div>

    </div>

    <form class="needs-validation" on:submit|preventDefault={login} method="POST">

        <div class="row g-3 justify-content-center mt-2">

            <div class="col-12 col-md-8 col-lg-5">
                <label for="input_email" class="form-label">Email</label>
                <input type="email" class="form-control shadow {error != undefined ? 'is-invalid': ''}" id="input_email" name="email" placeholder="Insira o email" bind:value={email} required>
            </div>

        </div>
        <div class="row g-3 justify-content-center mt-2 align-items-end">
            <div class="col-12 col-md-8 col-lg-5">
                <label for="input_password" class="form-label">Palavra Passe</label>
                <div class="input-group has-validation">
                    <input type={type} class="form-control d-inline shadow {error != undefined ? 'is-invalid': ''}" value={password} name="password" placeholder="Insira a Palavra Passe" on:input={e => password = e.target.value} required>
                    <button id="toggle_show_password" on:click={togglePassword} type="button" class="btn btn-outline-secondary bg-transparent shadow float-end p-1"><img id="toggle_show_password_img" class="changed_image" src="{password_img_button}" alt="Alternar mostrar Palavra Passe" height="35"></button>  
                    <div class="invalid-feedback">
                        {error}
                    </div>
                </div>
            </div>
        </div>

        <div class="row g-3 justify-content-center mt-3">

            <div class="col-12 col-md-4 col-lg-3 text-center">
                <input class="btn btn-primary w-100 shadow" type="submit" name="login" value="Login"/>
            </div>

        </div>

        <div class="row g-3 justify-content-center mt-2">

            <div class="col-12 col-md-8 col-lg-5 text-center">
                
                <!-- svelte-ignore a11y-label-has-associated-control -->
                <label>Ainda não tens conta? <a href="/sign-up">Registar</a></label>

            </div>

        </div>

    </form>

</main>