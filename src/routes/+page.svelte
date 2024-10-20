<script lang="ts">
	import { onMount } from "svelte";
    import fireColors from "./components/Colors.json";
    import Controls from "./components/Controls.svelte";
    import FireTd from "./components/FireTd.svelte";

    const fireRows = 128;
    const fireColumns = 128;
    const startPixel = fireColumns * fireRows - fireColumns;

    let firePixelIndex: number[] = new Array(fireColumns * fireRows);

    let rowArray: string[][] = [];

    const createFire = () => {
        let i = 0;
        for(i; i < fireColumns; i++) {
            firePixelIndex[startPixel + i] = fireColors.length - 1;
        }
    }

    const onIncrease = () => {
        let i = 0;
        const pixelIndexLength = firePixelIndex.length;

        for (i; i < fireColumns; i++) {
            const currentIntensity = firePixelIndex[startPixel + i];
            const increase = Math.floor(Math.random() * 5);
            const newIntensity = (currentIntensity + increase) < pixelIndexLength
                ? currentIntensity + increase
                : pixelIndexLength;

            firePixelIndex[startPixel + i] = newIntensity;

        }
    }

    const onDecrease = () => {
        let i = 0;

        for (i; i < fireColumns; i++) {
            const currentIntensity = firePixelIndex[startPixel + i];
            const decrease = Math.floor(Math.random() * 5);
            const newIntensity = (currentIntensity - decrease) > 0
                ? currentIntensity - decrease
                : 0;

            firePixelIndex[startPixel + i] = newIntensity;
        }
    }

    const destroyFire = () => {
        let i = 0;
        for(i; i < fireColumns; i++) {
            firePixelIndex[startPixel + i] = 0;
        }
    }


    const renderCols = (i: number) => {
        let cols: string[] = [];
        let j = 0;

        for (j; j < fireColumns; j++) {
            const pixelIntensityIndex = (i * fireColumns) + j;
            cols[j] = fireColors[firePixelIndex[pixelIntensityIndex]]
        }
        return cols;
    };

    const renderRows = () => {
        let rows: string[][] = []
        let i = 0;
        for(i; i < fireRows; i++) {
            rows[i] = renderCols(i);
        }
        return rows;
    };

    const updateFireIntensity = (currentPixelIndex: number) => {
        const belowPixelIndex = currentPixelIndex + fireColumns;
        if (belowPixelIndex >= (fireColumns * fireRows)) return;

        const decay = Math.floor(Math.random() * 3.0);
        const belowPixelFireIntensity = firePixelIndex[belowPixelIndex];
        const newFireIntensity = belowPixelFireIntensity - decay >= 0
            ? belowPixelFireIntensity - decay
            : 0;

        firePixelIndex[currentPixelIndex - decay] = newFireIntensity;
    };


    const calcFirePropagation = () => {
        let i = 0;
        for (i; i < fireRows; i++) {
            let j = 0;
            for (j; j < fireColumns; j++) {
                const pixelIntensityIndex = (fireColumns * i) + j;
                updateFireIntensity(pixelIntensityIndex)
            }
        }
        rowArray = renderRows();
    };

    const start = () => {
        createFire();
        rowArray = renderRows();

        setInterval(calcFirePropagation, 80);
    }

    onMount(() => {
        start();
    });
</script>

<style>
    .doom-fire {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100%;
        width: 100%;

        color: white;
        background-color: black;
    }
    .fire-table {
        border-collapse: collapse;
    }
</style>


<div class="doom-fire">
    <h1>Doom Fire Svelte Edition</h1>

    <div>
        <table class="fire-table">
            <tbody>
                {#each rowArray as row}
                    <tr>
                        {#each row as col}
                        <td
                            class="fire-pixel"
                            style={`background-color: #${col ?? '000000'}`}
                        ></td>
                        {/each}
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>

    <div>
        <Controls
            maxFire={createFire}
            increaseFire={onIncrease}
            decreaseFire={onDecrease}
            minFire={destroyFire}
        />
    </div>
</div>
