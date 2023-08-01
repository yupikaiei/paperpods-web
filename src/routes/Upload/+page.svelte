<script>
  import { onMount } from "svelte";
  import PodcastListItem from "../../components/PodcastListItem.svelte";

  let id;
  let file;
  let podcasts = [];
  let generating = false;
  let env = {
    PUBLIC_API_URL: "",
  };

  const getPodcasts = async () => {
    const response = await fetch(`${env.PUBLIC_API_URL}podcasts/${id}`, {
      method: "GET",
      headers: {
        "Access-Control-Allow-Origin": "*",
      },
    });

    const json = await response.json();
    console.log(json);
    podcasts = json;
  };

  onMount(async () => {
    id = sessionStorage.getItem("id");
    env = {
      PUBLIC_API_URL: sessionStorage.getItem("serverUrl"),
    };
    console.log(id);
    await getPodcasts();
  });

  const setFile = (event) => {
    file = event.target.files[0];
  };

  const handleInput = async () => {
    generating = true;

    const formData = new FormData();
    formData.append("podcastName", sessionStorage.getItem("podcastName"));
    formData.append("hostName", sessionStorage.getItem("hostName"));
    formData.append(
      "explanationLevel",
      sessionStorage.getItem("explanationLevel")
    );
    formData.append("voice_id", sessionStorage.getItem("voice_id"));
    formData.append("id", id);
    formData.append("pdf", file);

    // send file to server here
    const response = await fetch(`${env.PUBLIC_API_URL}upload`, {
      method: "POST",
      headers: {
        "Access-Control-Allow-Origin": "*",
      },
      body: formData,
    });

    const json = await response.json();
    podcasts = [json, ...podcasts];

    generating = false;
  };
</script>

<div class="flex flex-col w-full">
  <div class="grid h-48 card bg-base-300 rounded-box p-5">
    <div class="card-title pb-5">Upload Research Paper</div>
    <div class="flex flex-col space-y-4">
      {#if generating}
      <div class="flex justify-center">
        <span class="loading loading-spinner text-info"></span>
      </div>
      {:else}
        <input
          type="file"
          class="file-input file-input-bordered w-full max-w-xs"
          on:change={setFile}
        />
        <button
          class="btn btn-primary"
          on:click={handleInput}
          disabled={generating}>Generate Podcast</button
        >
      {/if}
    </div>
  </div>
  <div class="divider" />
  <div class="grid card bg-base-300 rounded-box p-5">
    <div class="card-title pb-5">Podcasts</div>
    <div class="flex flex-col space-y-4">
      {#each podcasts as podcast}
        <PodcastListItem
          name={podcast.name}
          file={`${env.PUBLIC_API_URL}${podcast.file}`}
          id={podcast.id}
          paperTitle={podcast.paperTitle}
          paperUrl={`${env.PUBLIC_API_URL}${podcast.paperUrl}`}
          host={podcast.host}
          image={podcast.image}
          created_at={podcast.created_at}
        />
        <!-- <div>
          <audio controls>
            <source src={`${PUBLIC_API_URL}${podcast.file}`} type="audio/mpeg" />
          </audio>
        </div> -->
      {/each}
    </div>    
  </div>
</div>
