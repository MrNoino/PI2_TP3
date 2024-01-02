<script>
// @ts-nocheck

    import { goto } from "$app/navigation";

    let data;
    let status;


    async function logout(){

        var url = new URL('http://127.0.0.1:5000/api/users/logout/');
        var response = await fetch(url, {
            method : 'PATCH',
            mode: 'cors',
            headers: {
            'Accept': 'application/json',
            'Authorization': sessionStorage.getItem("token")
            }
        });

        data = await response.json();

        status = response.status;

        if(status == 200){

            sessionStorage.removeItem("token");
            goto("/");

        }

    }

</script>

<header>

    <nav class="navbar navbar-dark bg-primary" data-bs-theme="light">

        <div class="container-fluid">
            <a class="navbar-brand" href="/">
                <img src="/assets/logo.jpeg" alt="Logo" width="30" height="30" class="d-inline-block align-text-top img-thumbnail">
                Food Saver
            </a>

            <button class="navbar-toggler" type="button" data-bs-toggle="offcanvas" data-bs-target="#menu" aria-controls="menu">
                <!-- svelte-ignore a11y-label-has-associated-control -->
                <span class="d-flex align-items-center"><img class="m-0 p-0" src="/assets/menu.svg" alt="Icon do menu" height="30"/><label class="mb-1">Menu</label></span>
            </button>
            <div class="offcanvas offcanvas-start black" data-bs-scroll="true" tabindex="-1" id="menu" aria-labelledby="menuLabel">
                <div class="offcanvas-header">
                    <h5 class="offcanvas-title" id="menuLabel">Food Saver - Menu</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
                </div>
                <div class="offcanvas-body">
                    <ul class="navbar-nav justify-content-end flex-grow-1 pe-3">
                        <li class="nav-item">
                            <a class="nav-link" href="/offers">Ofertas</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="/purchases">Compras</a>
                        </li>
                    </ul>
                </div>
                <div class="p-2 d-flex justify-content-end">

                    <button class="btn btn-danger" on:click={logout}>
                        <!-- svelte-ignore a11y-label-has-associated-control -->
                        <span class="d-flex align-items-center"><img src="/assets/exit.svg" alt="Icon de terminar sessão" height="30"/>Terminar Sessão<label class="mb-1"></label></span>
                    </button>

                </div>
            </div>
        </div>
    </nav>

</header>