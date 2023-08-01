<script>
    import { onMount } from "svelte";
    import "../app.css";

    let isAuthenticated = false;

    onMount(async () => {
        isAuthenticated = sessionStorage.getItem("id") !== null;
    });

    const logout = () => {
        sessionStorage.clear();
        window.location.href = "/";
    };

    const edit = () => {
        window.location.href = "/Setup";
    };
</script>

<div>
    {#if isAuthenticated}
        <div class="navbar bg-base-100">
            <div class="flex-1">
                <figure>
                    <img class="h-8"
                        src="logo.png"
                        alt="Logo"
                    />
                </figure>
                <a class="btn btn-ghost normal-case text-xl">PaperPods</a>
            </div>
            <div class="flex-none">
                <button class="btn btn-square btn-ghost" on:click={edit}>
                    <img class="h-5" src="edit-icon.svg" alt="Edit icon" />
                </button>
                <button class="btn btn-square btn-ghost" on:click={logout}>
                    <img class="h-5" src="logout.svg" alt="Edit icon" />
                </button>
            </div>
        </div>
    {/if}
    <div>
        <div class="spinner" />
    </div>
    <div>
        <slot />
    </div>
</div>
