<!-- create a registration page -->
<script>
    import {
        PUBLIC_API_URL
    } from '$env/static/public'

    let username = "";
    let password = "";
    let confirm_password = "";
    let openai_api_key = "";
    let elabs_api_key = "";
    let error = "";

    const goback = () => {
        history.back();
    };

    const register = async () => {
        if (password != confirm_password) {
            error = "Passwords do not match";
            return;
        }

        // validate if all fields are filled
        if (username == "" || password == "" || confirm_password == "") {
            error = "Please fill in all fields";
            return;
        }

        const formData = new FormData();
        formData.append("username", username);
        formData.append("password", password);
        formData.append("openai_api_key", openai_api_key);
        formData.append("elabs_api_key", elabs_api_key);

        const response = await fetch(`${PUBLIC_API_URL}/register`, {
            method: "POST",
            headers: {
                "Access-Control-Allow-Origin": "*",
            },
            body: formData,
        });

        const json = await response.json();
        if (json.error) {
            error = json.error;
        } else {
            window.location.href = "/";
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
                </div>
                <div class="form-control">
                    <label class="label">
                        <span class="label-text">Confirm Password</span>
                    </label>
                    <input
                        type="password"
                        placeholder="Confirm Password"
                        class="input input-bordered"
                        bind:value={confirm_password}
                    />
                    <label class="label">
                        {#if error}
                            <p class="error">{error}</p>
                        {/if}
                    </label>
                </div>
                <div class="form-control">
                    <label class="label">
                        <span class="label-text">Open AI API key</span>
                    </label>
                    <input
                        type="password"
                        placeholder="sk-..."
                        class="input input-bordered"
                        bind:value={openai_api_key}
                    />
                    <label class="label">
                        {#if error}
                            <p class="error">{error}</p>
                        {/if}
                    </label>
                </div>
                <div class="form-control">
                    <label class="label">
                        <span class="label-text">11Labs API key</span>
                    </label>
                    <input
                        type="password"
                        placeholder="..."
                        class="input input-bordered"
                        bind:value={elabs_api_key}
                    />
                    <label class="label">
                        {#if error}
                            <p class="error">{error}</p>
                        {/if}
                    </label>
                </div>
                <div class="form-control mt-6">
                    <button class="btn" on:click={goback}>
                        Back
                    </button>
                    <button class="btn btn-primary" on:click={register}>
                        Register
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
