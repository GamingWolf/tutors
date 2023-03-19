<script lang="ts">
  import { Avatar } from "@skeletonlabs/skeleton";
  import { onDestroy } from "svelte";
  import { Icon, Image } from "tutors-ui";
  import { convertMd } from "tutors-reader-lib/src/utils/markdown-utils";
  import { currentCourse, layout } from "tutors-reader-lib/src/stores/stores";
  import { cardTransition } from "../../../animations";
  import CardTitle from "./Components/CardTitle.svelte";

  export let title = "";
  export let target = "";
  export let type = "";
  export let route = "";
  export let video = "";
  export let summary = "";
  export let img = "";
  export let icon = "";
  export let miniImage = false;

  let borderColor: string = "";

  if (type === "web") {
    if (route.startsWith("http")) {
      target = "_blank";
    }
  } else if (type == "video") {
    route = video;
  }
  summary = convertMd(summary, "");

  if (type === "talk" || type === "course" || type === "topic" || type === "archive" || type === "paneltalk") {
    borderColor = "primary";
  } else if (type === "web" || type === "panelvideo" || type === "video") {
    borderColor = "error";
  } else if (type === "lab" || type === "unit" || type === "side") {
    borderColor = "success";
  } else if (type === "note" || type === "reference" || type === "github" || type === "panelnote") {
    borderColor = "warning";
  } 
  let headingText = "";
  let cardWidths = "";
  let imageHeight = "";
  let iconHeight: number;

  if (miniImage) {
    imageHeight = "h-10";
    iconHeight = 48;
  }

  const unsubscribe = layout.subscribe((layout) => {
    if (layout === "compacted") {
      headingText = "!text-md font-medium";
      cardWidths = "w-36 h-[18rem]";
    } else {
      headingText = "!text-lg font-semibold";
      cardWidths = "w-60 h-[24rem]";
    }

    if (!miniImage) {
      if (layout === "compacted") {
        iconHeight = 90;
        imageHeight = "h-20";
      } else {
        iconHeight = 180;
        imageHeight = "h-48";
      }
    }
  });

  onDestroy(unsubscribe);
</script>

<a href="{route}" target="{target}">
  <div transition:cardTransition class="card !bg-surface-50 dark:!bg-surface-700 border-y-8 border-{borderColor}-500 m-2 {cardWidths} transition-all hover:scale-105">
    <CardTitle title="{title}" type="{type}" video="{video}" textSize="{headingText}" />
    <div class="card-body">
      <figure class="flex justify-center object-scale-down p-1">
        {#if icon !== ""}
        <Icon type="{icon}" />
        {:else}
        <Avatar src="{img}" alt="{title}" width="{imageHeight}" rounded="rounded-xl" background="none" />
        {/if}
      </figure>
    </div>
    <footer class="card-footer">
      <div class="prose dark:prose-invert line-clamp-3 text-center leading-6">
        {@html summary}
      </div>
    </footer>
  </div>
</a>
