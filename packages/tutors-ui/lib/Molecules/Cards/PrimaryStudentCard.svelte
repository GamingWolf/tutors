<script lang="ts">
  import { Avatar } from "@skeletonlabs/skeleton";
  import Iconify from "@iconify/svelte";
  import { StudentLoEvent } from "tutors-reader-lib/src/types/metrics-types";
  import { layout } from "tutors-reader-lib/src/stores/stores";
  import { cardTransition } from "../../animations";

  export let lo: StudentLoEvent;
  let imageHeight = "h-32";
  let iconHeight = "80";
  let headingText = "";
  let cardWidths = "";

  const unsubscribe = layout.subscribe((layout) => {
    if (layout === "compacted") {
      headingText = "!text-md font-medium";
      cardWidths = "w-36 h-[21rem]";
      iconHeight = "80";
    } else {
      headingText = "!text-lg font-semibold";
      cardWidths = "w-60 h-[25rem]";
      iconHeight = "150";
    }
  });
</script>

<a href="https://next.tutors.dev{lo.loRoute}" target="_blank" rel="noreferrer">
  <div transition:cardTransition class="card !bg-surface-50 dark:!bg-surface-700 border-y-8 border-primary-500 m-2 w-56 {cardWidths} transition-all hover:scale-105">
    <!-- <div class="card h-90 border-primary-500 m-2 w-4/5 overflow-x-hidden border-y-8 transition-all hover:scale-105 {cardWidths}"> -->
    <div class="flex">
      <header class="card-header inline-flex items-center">
        <Avatar src="{lo.studentImg}" alt="{lo.studentName}" class="mr-2" />
        <h6>{lo.studentName}</h6>
      </header>
    </div>
    <div class="card-body">
      <div class="my-2 justify-center">
        {#if lo.loIcon}
          <Iconify icon="{lo.loIcon.type}" color="{lo.loIcon.color}" height="{iconHeight}" />
        {:else}
          <img loading="lazy" class="mx-auto {imageHeight}" src="{lo.loImage}" alt="{lo.loTitle}" />
        {/if}
      </div>
    </div>
    <footer class="card-footer text-center">
      <p class="mt-2 mb-2 font-semibold">
        <span class="italic">{lo.courseTitle}</span>
      </p>
      <hr />
      <p class="mt-2">
        <span class="italic">{lo.loTitle}</span>
      </p>
    </footer>
  </div>
</a>
