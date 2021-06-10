<script lang="ts">
  type Tab = string;

  let defaultTab = 'https://pukmajster.github.io/bunker'

  let tabs: Tab[] = [
    defaultTab,
    'https://svelte.dev/tutorial/writable-stores',
  ];

  let focusedTabIndex = 0;

  function focusTabByIndex(index: number) {
    focusedTabIndex = index;
  }

  function appendTab() {
    tabs = [...tabs, defaultTab];
    focusTabByIndex(tabs.length - 1);
  }

  function removeByIndex(e, index: number) {
    e.preventDefault();
    tabs.splice(index, 1);
    tabs = tabs;
  }

  function updateTabByIndex(e, index: number) {
    console.log(e.target.value);
    
    tabs[index] = e.target.value;
    tabs = tabs;
  }

</script>

<div class="root">
  <div class="toolbar">

    {#each tabs as tab, i} 
      <input
        class:focused={i == focusedTabIndex}
        on:blur={e => updateTabByIndex(e, i)}
        value={tab}
        on:keydown={e => {
          if(e.keyCode == 13) {
            updateTabByIndex(e, i)
          }
        }}
        on:contextmenu={(e) => removeByIndex(e, i)}
        on:click={() => focusTabByIndex(i)}
      />
        
    {/each}

    <button on:click={appendTab} >+</button>

  </div>

  <div class="toolbar_blur"></div>


  {#each tabs as tab, i} 
    <iframe class:focused={i == focusedTabIndex} src={tab} title={tab} />
  {/each}


</div>

<style lang="scss">
  .toolbar {

    position: fixed;
    top: 0;
    right: 0;
    left: 0;

    height: 52px;
    padding: 9px;

    display: flex;
    gap: 5px;
    /* overflow-x: scroll; */

    opacity: 0;
    background-color: rgba(189, 189, 189, 0.193);

    transform: translateY(-28px);

    transition: all 0.12s ease;
    transition-delay: 0.3s;
  }

  .toolbar:hover {
    transform: translateY(0);
    opacity: 1;
  }


  .toolbar_blur {
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;

    backdrop-filter: blur(25px);

    height: 100%;
    height: 52px;

    pointer-events: none;
    z-index: -1;

    opacity: 0;

    transition: all 0.12s ease;
    transition-delay: 0.3s;
  }

  .toolbar:hover + .toolbar_blur {
    opacity: 1;
  }

  iframe {
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;

    width: 100%;
    height: 100%;

    border: 0;
    outline: 0;

    display: none;
    z-index: -5;

  }

  iframe.focused {
    display: block;
  }



  input, button {
    padding: 9px;
    background: none;

    color: white;
    border: none;
    outline: none;
    box-shadow: none;

    background-color: rgba(0, 0, 0, 0.254);
    min-width: 32px;

    border-radius: 4px;

    &:focus, &:hover {
      background-color: rgba(0, 0, 0, 0.357);
    }
  }

  input.focused {
    background-color: rgba(0, 0, 0, 0.611);
  }

  input {
    min-width: 120px;
  }
</style>