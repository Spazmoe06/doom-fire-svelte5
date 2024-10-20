<script lang="ts">
    interface Controls {
        maxFire: VoidFunction;
        increaseFire: VoidFunction
        decreaseFire: VoidFunction,
        minFire: VoidFunction
    }

    let {maxFire, increaseFire, decreaseFire, minFire}:Controls = $props()
</script>

<div>
    <button onclick={maxFire}>Max ++</button>
    <button onclick={increaseFire}>Increase +</button>
    <button onclick={decreaseFire}>Decrease -</button>
    <button onclick={minFire}>Min --</button>
</div>
