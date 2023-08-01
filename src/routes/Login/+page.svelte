<!-- create a login page -->
<script>
    import { onMount } from "svelte";

    let username = "";
    let password = "";
    let error = "";
    let env = {
        PUBLIC_API_URL: "",
    };

    onMount(async () => {
        env = {
            PUBLIC_API_URL: sessionStorage.getItem("serverUrl"),
        };
        console.log('asd:', env.PUBLIC_API_URL)
        if (
            sessionStorage.getItem("id") !== null &&
            sessionStorage.getItem("podcast") !== undefined
        ) {
            window.location.href = "/Upload";
        } else if (sessionStorage.getItem("id") !== null) {
            window.location.href = "/Setup";
        }
    });

    const login = async () => {
        const formData = new FormData();
        formData.append("username", username);
        formData.append("password", password);

        const response = await fetch(`${env.PUBLIC_API_URL}login`, {
            method: "POST",
            headers: {
                "Access-Control-Allow-Origin": "*",
            },
            body: formData,
        });

        console.log(response);

        const json = await response.json();
        if (json.error) {
            error = json.error;
        } else {
            sessionStorage.setItem("username", json.username);
            sessionStorage.setItem("id", json.id);
            sessionStorage.setItem("podcastName", json.podcastName);
            sessionStorage.setItem("hostname", json.hostname);
            sessionStorage.setItem("explanationLevel", json.explanationLevel);
            sessionStorage.setItem("voice_id", json.voice_id);

            if (json.podcast != null) {
                window.location.href = "/Upload";
                return;
            }

            window.location.href = "/Setup";
        }
    };
</script>

<div class="hero min-h-screen bg-base-200">
    <div class="hero-content flex-col lg:flex-row-reverse">
        <div class="text-center lg:text-left">
            <div class="flex justify-center">
                <figure>
                    <img src="logo.png" alt="Logo" />
                </figure>
            </div>
            <h1 class="text-5xl font-bold">PaperPods</h1>
            <p class="py-6">
                Turn research papers into your own custom podcasts
            </p>
        </div>
        <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
            <div class="card-body">
                <div class="form-control">
                    <label class="label">
                        <span class="label-text">Username</span>
                    </label>
                    <input
                        type="text"
                        placeholder="Username"
                        class="input input-bordered"
                        bind:value={username}
                    />
                </div>
                <div class="form-control">
                    <label class="label">
                        <span class="label-text">Password</span>
                    </label>
                    <input
                        type="password"
                        placeholder="Password"
                        class="input input-bordered"
                        bind:value={password}
                    />
                    <label class="label">
                        {#if error}
                            <p class="error">{error}</p>
                        {/if}
                    </label>
                </div>
                <div class="form-control mt-6">
                    <a href="/Register" class="btn btn">Sign up</a>
                    <button class="btn btn-primary" on:click={login}
                        >Sign in
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>

<style>
    .error {
        color: red;
    }
</style>
