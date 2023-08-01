<script>
    import VoiceListElement from "./VoiceListElement.svelte";

    export let voices = [];
    export let setVoice = () => {};

    let searchTerm = "";
    let filteredVoices = [];

    const handleSearch = (event) => {
        searchTerm = event.target.value;
    };

    $: filteredVoices = voices.filter((voice) => {
        searchTerm = searchTerm.toLowerCase();
        let terms = searchTerm.split(" ");

        let validation = true;

        terms.forEach((element) => {
            voice.labels.description =
                voice.labels.description != undefined
                    ? voice.labels.description
                    : "";

            let descriptionComparison = voice.labels.description
                .toLowerCase()
                .includes(element);

            validation =
                voice != undefined
                    ? voice.name.toLowerCase().includes(element) ||
                      voice.labels.age.includes(element) ||
                      voice.labels.gender.toLowerCase().includes(element) ||
                      descriptionComparison
                    : false;
            console.log(validation);
            if (validation) {
                return;
            }
        });

        return validation;

        // if voice has description, check if it contains search term
    });
</script>

<div class="pb-5">
    <input
        type="text"
        placeholder="Search"
        class="input input-bordered input-primary w-full max-w-xs"
        on:input={handleSearch}
    />
</div>
<div class="voice-list">
    <div class="wrapper">
        {#each filteredVoices as voice}
            <VoiceListElement
                name={voice.name}
                age={voice.labels.age}
                gender={voice.labels.gender}
                accent={voice.labels.accent}
                description={voice.labels.description}
                audioSrc={voice.preview_url}
                onClick={setVoice.bind(this, voice)}
            />
        {/each}
    </div>
</div>
