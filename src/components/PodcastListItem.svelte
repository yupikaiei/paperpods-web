<script>
    import { Flex } from "@svelteuidev/core";

    export let id;
    export let name;
    export let paperTitle;
    export let paperUrl;
    export let host;
    export let image;
    export let file;
    export let created_at;

    const randomCover = () => {
        const covers = ["cover1.png", "cover2.png", "cover3.png"];

        return covers[Math.floor(Math.random() * covers.length)];
    };

    let cover = randomCover();
</script>

<div>
    <Flex>
        <div>
            <div class="img">
                <img src={cover} />
            </div>
        </div>
        <div class="info">
            <h3><a href={paperUrl}>{paperTitle}</a></h3>
            <h4>{name} with {host}</h4>
            <h4>Uploaded on {created_at}</h4>
        </div>
    </Flex>
    <div class="actions">
        <audio controls>
            <source src={file} type="audio/mpeg" />
            Your browser does not support the audio element.
        </audio>
    </div>
    <div class="divider"></div> 
</div>

<style>
    .img {
        width: 5rem;
    }
    .info {
        padding-left: .5rem;
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
