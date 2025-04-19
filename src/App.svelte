<script lang="ts">
  import "@fontsource-variable/inter";

  import html2canvas from "html2canvas";

  import Tweet from "./components/Tweet.svelte";
  import Button from "./components/ui/button/button.svelte";
  import Checkbox from "./components/ui/checkbox/checkbox.svelte";
  import DatePicker from "./components/ui/date-picker/date-picker.svelte";
  import Input from "./components/ui/input/input.svelte";
  import Textarea from "./components/ui/textarea/textarea.svelte";

  let name = "ThePrimeagen";
  let handle = "ThePrimeagen";
  let pfpUrl =
    "https://pbs.twimg.com/profile_images/1759330620160049152/2i_wkOoK_400x400.jpg";
  let caption = "i put vim on your mom's computer";
  let broadcastText = "Twitter Web App";
  let tweetCreatedAt = new Date().toISOString();
  let isVerified = true;

  let tweet: HTMLElement;

  async function generateScreenshot() {
    const canvas = await html2canvas(tweet, {
      useCORS: true,
      scale: 2,
    });

    return canvas;
  }

  async function downloadImage() {
    const canvas = await generateScreenshot();
    const url = canvas.toDataURL();

    const downloadAnchor = document.createElement("a");

    downloadAnchor.style.display = "none";
    downloadAnchor.href = url;
    downloadAnchor.download = "";
    downloadAnchor.click();

    document.body.appendChild(downloadAnchor);
    document.removeChild(downloadAnchor);
  }

  async function copyScreenShot() {
    const canvas = await generateScreenshot();

    canvas.toBlob((blob) => {
      if (!blob) return;

      navigator.clipboard.write([new ClipboardItem({ "image/png": blob })]);
    }, "image/png");
  }
</script>

<h1
  class="scroll-m-20 text-4xl text-center py-10 font-extrabold tracking-tight lg:text-5xl"
>
  Tweet Screenshot
</h1>
<div class="flex gap-10 px-40">
  <div class="flex flex-col gap-2 w-full">
    <Input type="text" bind:value={name} placeholder="Name" />
    <Input type="text" bind:value={handle} placeholder="Handle" />
    <Input type="text" bind:value={pfpUrl} placeholder="Profile Picture URL" />
    <Input type="text" bind:value={broadcastText} placeholder="Platform Text" />
    <Textarea bind:value={caption} placeholder="Caption" />
    <div class="flex gap-2 items-center">
      <p class="leading-7 [&:not(:first-child)]:mt-4">Verified?</p>
      <Checkbox
        on:click={() => (isVerified = !isVerified)}
        checked={isVerified}
      />
    </div>
    <p class="leading-7 [&:not(:first-child)]:mt-4">
      Select Tweet creation date
    </p>
    <DatePicker
      onDatePicked={(date) =>
        (tweetCreatedAt = date?.toString() ?? new Date().toISOString())}
    />
  </div>
  <div class="flex flex-col items-end">
    <div id="tweet" class="bg-black" bind:this={tweet}>
      <Tweet
        {name}
        {handle}
        {pfpUrl}
        {caption}
        {broadcastText}
        {isVerified}
        createdAt={tweetCreatedAt}
      />
    </div>
    <div class="mt-4">
      <Button on:click={downloadImage}>Download</Button>
      <Button variant="secondary" on:click={copyScreenShot}>Copy</Button>
    </div>
  </div>
</div>
