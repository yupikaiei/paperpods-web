<script>
    import { onMount } from "svelte";
    import VoiceInfo from "../../components/VoiceInfo.svelte";
    import VoiceSelectionList from "../../components/VoiceSelectionList.svelte";

    let podcastName = "";
    let hostName = "";
    let explanationLevel = ["child", "teenager", "adult"];
    let explanationLevelSelected = "teenager";
    let show = false;
    let voices = [];
    let voiceSelected = null;
    let error = "";
    let env = {
        PUBLIC_API_URL: "",
    };

    onMount(() => {
        env = {
            PUBLIC_API_URL: sessionStorage.getItem("serverUrl"),
        };
        podcastName = sessionStorage.getItem("podcastName") !== 'null' ? sessionStorage.getItem("podcastName") : "";
        hostName = sessionStorage.getItem("hostName") !== 'null' ? sessionStorage.getItem("hostName") : "";
        explanationLevelSelected = sessionStorage.getItem("explanationLevel") !== 'null' ? sessionStorage.getItem("explanationLevel") : "teenager";
        voiceSelected = sessionStorage.getItem("voice") !== 'null' ? JSON.parse(sessionStorage.getItem("voice")) : null;
        getVoices();
    });

    const storeInSession = async () => {
        // check if all fields are filled
        if (podcastName == "" || hostName == "" || voiceSelected == "") {
            error = "Please fill in all fields";
            return;
        }

        sessionStorage.setItem("podcastName", podcastName);
        sessionStorage.setItem("hostName", hostName);
        sessionStorage.setItem("explanationLevel", explanationLevelSelected);
        sessionStorage.setItem("voice_id", voiceSelected.voice_id);
        sessionStorage.setItem("voice", voiceSelected);

        // update user in database
        const formData = new FormData();
        formData.append("id", sessionStorage.getItem("id"));
        formData.append("podcastName", podcastName);
        formData.append("hostName", hostName);
        formData.append("explanationLevel", explanationLevelSelected);
        formData.append("voice_id", voiceSelected.voice_id);
        formData.append("openai_api_key", sessionStorage.getItem("openai_api_key"));
        formData.append("elabs_api_key", sessionStorage.getItem("elabs_api_key"));

        const res = await fetch(`${env.PUBLIC_API_URL}update`, {
            method: "POST",
            headers: {
                "Access-Control-Allow-Origin": "*",
            },
            body: formData,
        });

        const json = await res.json();
        console.log(json);
        if (res.status != 200) {
            alert("Error updating user");
            return;
        }

        // navigate to upload page
        window.location.href = "/Upload";
    };

    const getVoices = async () => {
        let res = await fetch(`${env.PUBLIC_API_URL}/voices`);
        const json = await res.json();
        voices = json.voices;
        console.log(voices);
    };

    const selectVoice = () => {
        show = !show;
    };

    const setVoice = (voice) => {
        console.log(voice);
        voiceSelected = voice;
        selectVoice();
    };
</script>

<div style="display: {!show ? 'flex' : 'none'};">
    <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
        <div class="card-body">
            <div class="form-control">
                <label class="label">
                    <span class="label-text">Podcast name</span>
                </label>
                <input
                    type="text"
                    placeholder="ex. Something cool"
                    class="input input-bordered"
                    bind:value={podcastName}
                />
            </div>
            <div class="form-control">
                <label class="label">
                    <span class="label-text">Name of the host</span>
                </label>
                <input
                    type="text"
                    placeholder="ex. John Doe"
                    class="input input-bordered"
                    bind:value={hostName}
                />
            </div>
            <div class="form-control">
                <div>
                    {#if voiceSelected}
                        <VoiceInfo
                            name={voiceSelected.name}
                            age={voiceSelected.labels.age}
                            gender={voiceSelected.labels.gender}
                            accent={voiceSelected.labels.accent}
                            description={voiceSelected.labels.description}
                        />
                    {/if}
                    <button
                        class="btn btn-primary"
                        type="button"
                        on:click={selectVoice}
                    >
                        Select Voice
                    </button>
                </div>
            </div>
            <div class="form-control">
                <label class="label">
                    <span class="label-text">Explanation Level</span>
                </label>
                <select
                    class="select select-bordered w-full max-w-xs"
                    bind:value={explanationLevelSelected}
                >
                    {#each explanationLevel as level}
                        <option value={level}>{level}</option>
                    {/each}
                </select>
            </div>
            <label class="label">
                {#if error}
                    <p class="error">{error}</p>
                {/if}
            </label>
            <div class="form-control mt-6">
                <button
                    class="btn btn-primary"
                    type="submit"
                    on:click={storeInSession}
                >
                    Create
                </button>
            </div>
        </div>
    </div>
</div>
<div style="display: {show ? 'block' : 'none'};" >
    <VoiceSelectionList
        voices={voices}
        setVoice={setVoice}
    />
    <!-- <div class="voice-list">
        <div class="wrapper">
            {#each voices as voice}
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
    </div> -->
</div>

<style>
    .error {
        color: red;
    }
</style>
