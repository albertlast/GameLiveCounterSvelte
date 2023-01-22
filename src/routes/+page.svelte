<script lang="ts">
  import PlayerView from './components/PlayerView.svelte'
  import SettingView from './components/SettingView.svelte'
  import ReSettings5Line from './icons/ReSettings5Line.svelte'

  let showSettings: boolean = false
</script>

<main class="w-screen h-screen">
  <div
    id="openSettings"
    class="openSetting"
    on:click={() => (showSettings = !showSettings)}
    on:keypress={() => (showSettings = !showSettings)}
  >
    <ReSettings5Line />
  </div>
  <SettingView {showSettings} on:click={() => (showSettings = !showSettings)} />
  <PlayerView playerName="first" class="PlayerView rotate-180 bg-cyan-900" style="height: 50vh" />
  <PlayerView playerName="second" class="PlayerView bg-yellow-600" style="height: 50vh" />
</main>

<style>
  main {
    color: rgba(235, 235, 235, 0.64);
    user-select: none;
    -webkit-user-select: none;
  }
  /* :global(.visible) {
    opacity: 1;
  }
  :global(.invisible) {
    opacity: 0;
  } */

  :global(.PlayerView) {
    font-size: min(28vh, 30vw);
  }
  .openSetting {
    position: absolute;
    top: 50%;
    right: 50%;
    transform: translate(50%, -50%);
    z-index: 5;
    font-size: min(14vh, 14vw);
  }
</style>
