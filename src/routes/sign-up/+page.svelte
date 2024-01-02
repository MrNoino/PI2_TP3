<svelte:head>
    <title>Food Saver - Registar</title>
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

    let name = "";
    let email = "";
    let password = "";
    let phone_number = "";
    let error = undefined;

    let show_password = false;
    let type = show_password ? "text" : "password";
    let password_img_button = show_password ? "/assets/eye-close.svg" : "/assets/eye.svg";

    function togglePassword(){

        show_password = !show_password;
        type = show_password ? "text" : "password";
        password_img_button = show_password ? "/assets/eye-close.svg" : "/assets/eye.svg";

    }


    async function signup(){

        var url = new URL('http://127.0.0.1:5000/api/users/');
        var response = await fetch(url, {
            method : 'POST',
            mode: 'cors',
            headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                name: name,
                email: email,
                password: password,
                phone_number: phone_number
            })
        });

        var data = await response.json();

        if(response.status == 200){

            error = undefined;
            goto("/");

        }else{
    
            error = data.message;

        }

    }

</script>

<main class="container mb-5">

    <div class="row justify-content-center mt-3">

        <div class="col-12 col-md-8 col-lg-6 text-center">

            <img src="/assets/logo.jpeg" class="rounded mx-auto size-inherit" alt="">

        </div>

    </div>

    <div class="row justify-content-center mt-3">

        <div class="col-12 col-md-8 col-lg-6 text-center">

            <h2 class="text-primary">Registar Conta</h2>

        </div>

    </div>

    <form class="needs-validation" on:submit|preventDefault={signup} method="POST">

        <div class="row g-3 justify-content-center mt-2">

            <div class="col-12 col-md-8 col-lg-5">
                <label for="input_name" class="form-label">Nome</label>
                <input type="text" class="form-control shadow {error != undefined ? 'is-invalid': ''}" id="input_name" name="name" placeholder="Insira o nome" bind:value={name} required>
            </div>

        </div>

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
                </div>
            </div>
        </div>

        <div class="row g-3 justify-content-center mt-2">

            <div class="col-12 col-md-8 col-lg-5">
                <label for="input_phone_number" class="form-label">Telefone</label>
                <div class="input-group has-validation">
                    <input type="tel" id="input_phone_number" name="phone_number" pattern="[0-9]{'{'}9{'}'}" maxlength="9" inputmode="numeric" class="form-control shadow {error != undefined ? 'is-invalid': ''}" placeholder="Insira o telefone" bind:value={phone_number} required>
                    <div class="invalid-feedback">
                        {error}
                    </div>
                </div>
            </div>
        </div>

        <div class="row g-3 justify-content-center mt-3">

            <div class="col-12 col-md-4 col-lg-3 text-center">
                <input class="btn btn-primary w-100 shadow" type="submit" name="signup" value="Registar"/>
            </div>

        </div>

        <div class="row g-3 justify-content-center mt-2">

            <div class="col-12 col-md-8 col-lg-5 text-center">
                
                <!-- svelte-ignore a11y-label-has-associated-control -->
                <label>Já tens conta? <a href="/">Iniciar Sessão</a></label>

            </div>

        </div>

    </form>

</main>