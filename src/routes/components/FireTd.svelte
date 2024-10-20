<script lang="ts">
	import type { Color } from "../model";

    interface FireTdProps {
        fireColor: string;
    }

    let {fireColor}: FireTdProps = $props();
</script>
<style>
    .fire-pixel {
        border: none;
    }
</style>
<td
    class="fire-pixel"
    style={`background-color: #${fireColor ?? '000000'}`}
></td>
