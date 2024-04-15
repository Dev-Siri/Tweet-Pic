<script lang="ts">
  import Verified from "./icons/Verified.svelte";

  export let name: string;
  export let handle: string;
  export let pfpUrl: string;
  export let broadcastText: string;
  export let createdAt: string;
  export let caption: string;
  export let isVerified: boolean;

  function getTweetCreatedDate(dateString: string) {
    const TWELVE_HOUR_BREAKPOINT = 12;

    const date = new Date(dateString);
    const month = date.toLocaleString("en-US", { month: "short" });
    const hour = date.getHours();
    const formattedHour =
      hour % TWELVE_HOUR_BREAKPOINT || TWELVE_HOUR_BREAKPOINT;
    const meridiem = hour < TWELVE_HOUR_BREAKPOINT ? "AM" : "PM";
    const minutes = date.getMinutes().toString();
    const day = date.getDate();
    const year = date.getFullYear();

    return `${formattedHour}:${minutes.padStart(
      2,
      "0"
    )} ${meridiem} · ${month} ${day}, ${year}`;
  }
</script>

<div role="presentation" class="p-4 h-fit rounded-lg w-[500px] select-none">
  <div class="flex">
    <img
      src={pfpUrl}
      alt="{name}'s Profile Picture"
      height="56"
      width="56"
      class="h-14 w-14 rounded-full"
    />
    <div class="ml-2">
      <div class="flex items-center justify-center gap-1">
        <p class="font-bold text-lg">{name}</p>
        {#if isVerified}
          <span class="text-twitter-blue">
            <Verified height={20} width={20} />
          </span>
        {/if}
      </div>
      <p class="text-gray-500">@{handle}</p>
    </div>
  </div>
  <p class="text-xl py-4">{caption}</p>
  <p class="text-gray-500">
    {getTweetCreatedDate(createdAt)} ·
    <span class="text-twitter-blue">{broadcastText}</span>
  </p>
</div>
