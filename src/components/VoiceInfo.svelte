<script>
    import { Flex } from "@svelteuidev/core";

    export let name;
    export let age;
    export let gender;
    export let accent;
    export let description;
</script>

<div class="wrapper">
    <Flex>
        <div class="avatar">
            <div>
                {#if gender == "male"}
                    <img class="img" src="male-avatar.png" />
                {:else}
                    <img class="img" src="female-avatar.png" />
                {/if}
            </div>
        </div>
        <div class="info">
            <h3>{name}</h3>
            <h4>{age} {gender} {accent}</h4>
            <h4>{description}</h4>
        </div>
    </Flex>
</div>

<style>
    .wrapper {
        padding: .5rem;
        border: 1px solid #ccc;
        border-radius: 0.5rem;
        margin-bottom: 1rem;
    }

    .img {
        width: 5rem;
    }

    .info {
        padding-left: 0.5rem;
    }

    .info h3 {
        font-size: 1.5rem;
    }

    .info h4 {
        font-size: 0.8rem;
    }
</style>
