<script>
    import { Flex } from "@svelteuidev/core";

    export let name;
    export let age;
    export let gender;
    export let accent;
    export let description;
    export let audioSrc;
    export let onClick;

    const clickHandler = () => {
        console.log("clicked");
        onClick();
    };
</script>

<div>
    <Flex>
        <div class="avatar stack">
            <div class="w-12">
                {#if gender == "male"}
                    <img
                        src="male-avatar.png"
                    />
                {:else}
                    <img
                        src="female-avatar.png"
                    />
                {/if}
            </div>
        </div>
        <div class="info">
            <h3>{name}</h3>
            <h4>a {age} {gender} {accent}</h4>
            <h4>Type: {description}</h4>
        </div>
    </Flex>
    <div class="actions">
        <audio controls>
            <source src={audioSrc} type="audio/mpeg" />
            Your browser does not support the audio element.
        </audio>
        <button class="btn btn-primary btn-xs btn-block" on:click={clickHandler}>Select</button>
    </div>
    <div class="divider"></div>
</div>

<style>
    .avatar {
        width: 100px;
        height: 100px;
    }
    .info {
        padding: 1rem;
    }

    .info:hover {
        background-color: azure;
    }

    .actions {
        display: flex;
        justify-content: space-between;
        align-items: flex-end;
        gap: 1rem;
    }

    .info h4 {
        font-size: small;
    }

    .info h3 {
        font-size: large;
    }

    audio {
        padding-top: 1rem;
    }
</style>
