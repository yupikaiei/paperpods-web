<!-- create a login page -->
<script>
    import { env } from "$env/dynamic/public";

    let serverUrl = "";
    let error = "";

    const validate = () => {
        if (serverUrl == "") {
            error = "Please fill in all fields";
            return;
        }

        // make sure it ends with a slash
        if (serverUrl[serverUrl.length - 1] != "/") {
            serverUrl += "/";
        }

        // if there's no env variable store in session
        if(env.PUBLIC_API_URL == undefined) {
            sessionStorage.setItem("serverUrl", serverUrl); 
        }
        else {
            sessionStorage.setItem("serverUrl", env.PUBLIC_API_URL); 
        }
    
        window.location.href = "/Login";
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
                        <span class="label-text">Server URL</span>
                    </label>
                    <input
                        type="text"
                        placeholder="https://api.paperpods.xyz/"
                        class="input input-bordered"
                        bind:value={serverUrl}
                    />
                </div>
                <div>
                    <p class="error">{error}</p>
                </div>
                <div class="form-control mt-6">
                    <button class="btn btn-primary" on:click={validate}
                        >Submit
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
