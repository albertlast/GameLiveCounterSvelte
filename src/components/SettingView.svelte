<script lang="ts">
    import { onMount } from 'svelte'
    import ReCheckboxBlankLine from '../icons/ReCheckboxBlankLine.svelte'
    import ReCheckboxLine from '../icons/ReCheckboxLine.svelte'
  
    export let showSettings: boolean = false
    let FullscreenEnabled: boolean = false
    let ScreenOffEnabled: boolean = false
    let wakeLock: null | AbortController = null
    let errorSceenOff: string | null = null
    let ScreenOffExists: boolean = false
    let classy: string = $$props.class
  
    async function requestWakeLock() {
      ScreenOffEnabled != ScreenOffEnabled
      if (ScreenOffEnabled) {
        try {
          wakeLock = await navigator.wakeLock.request('screen')
          wakeLock.addEventListener('release', (e) => {
            console.log(e)
            console.log('Wake Lock was released')
            // errorSceenOff.value = 'Wake Lock was released'
  
            ScreenOffEnabled = false
          })
          // errorSceenOff.value = 'Wake Lock is active'
          console.log('Wake Lock is active')
        } catch (e) {
          // errorSceenOff.value = `${e.name}, ${e.message}`
          console.error(`${e.name}, ${e.message}`)
        }
      } else {
        wakeLock.release()
        wakeLock = null
      }
    }
  
    function FullscreenChange() {
      FullscreenEnabled = !FullscreenEnabled
      if (FullscreenEnabled) document.documentElement.requestFullscreen()
      else document.exitFullscreen()
    }
    onMount(() => {
      ScreenOffExists = typeof navigator.wakeLock === 'object'
    })
  </script>
  
  {#if showSettings}
    <div id="setting" class="{classy} setting bg-red-800 flex align-middle flex-col">
      <div>Fullscreen</div>
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <div on:click|capture={FullscreenChange}>
        {#if !FullscreenEnabled}
          <ReCheckboxBlankLine />
        {:else}
          <ReCheckboxLine />
        {/if}
      </div>
      {#if ScreenOffExists}
        <div>Screen-Off</div>
        <div on:click|capture={requestWakeLock}>
          {#if !ScreenOffEnabled}
            <ReCheckboxBlankLine />
          {:else}
            <ReCheckboxLine />
          {/if}
        </div>
      {/if}
  
      <!-- <input type="checkbox" v-model="ScreenOffEnabled" class="w-12 h-12" /> -->
      {#if errorSceenOff}
        <div>{errorSceenOff}</div>
      {/if}
      <div on:click>close</div>
    </div>
  {/if}
  
  <style>
    .setting {
      position: absolute;
      inset: 0;
      z-index: 10;
      font-size: min(14vh, 14vw);
    }
  </style>
  