<script lang="ts">
    import { onMount } from 'svelte'
    export let playerName: string
    let classy: string = $$props.class
    let styley: string = $$props.style
  
    const storagePath = `glc${playerName}.live`
  
    let ls: Storage | null
  
    $: startLive = !!ls && Number.parseInt(ls.getItem(storagePath) ?? '20')
    $: CurrentLive = startLive
  
    let partValueShow: boolean = false
    let partValueTimeOutID: NodeJS.Timeout | null = null
    let partValueVal: number = 0
  
    $: updatePartValue = async (val: number) => {
      CurrentLive += val
      ls.setItem(storagePath, CurrentLive.toString())
      if (partValueShow === false) {
        partValueVal = 0
        partValueShow = true
      }
      partValueVal += val
      if (partValueTimeOutID) clearTimeout(partValueTimeOutID)
      partValueTimeOutID = setTimeout(() => {
        partValueShow = false
      }, 2000)
    }
  
    $: reduceLive = () => {
      updatePartValue(-1)
    }
    $: addLive = () => {
      updatePartValue(1)
    }
  
    onMount(() => {
      typeof localStorage !== `undefined` && (ls = localStorage)
    })
  </script>
  
  <div class="{classy} grid grid-cols-3 w-full" style={styley}>
    <div on:click={() => reduceLive()} on:keypress={() => reduceLive()} class="flex items-center">
      -
    </div>
    <div class="grid">
      <div class="{!partValueShow ? 'invisible' : ''} + ' ' + small">
        {partValueVal}
      </div>
      <div>{CurrentLive}</div>
    </div>
    <div
      on:click={() => addLive()}
      on:keypress={() => addLive()}
      class="flex items-center justify-end"
    >
      +
    </div>
  </div>
  
  <style>
    .small {
      font-size: min(5vh, 10vw);
    }
  </style>
  