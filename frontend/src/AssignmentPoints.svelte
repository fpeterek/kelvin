    <script>
        import {fetch} from './api.js'

        export let submits;
        export let link;
        export let color;
        export let assigned_points;
        export let login;
        export let task;
        export let submit_id;

        let show = false;
        let saving = false;
        let container;
        let value = assigned_points;

        function keydown(e) {
            if(e.code == 'Escape') {
                show = false;
            }
        }

        function click(e) {
            if(e.srcElement.classList.contains('overlay') || e.srcElement.classList.contains('inner')) {
                show = false;
            }
        }

        function ctxMenu(e) {
            if(window.innerWidth < 768) {
                e.preventDefault();
                show = true;
            }
        }

        async function save() {
            saving = true;
            const form = new FormData();
            form.append('assigned_points', value);

            await fetch(`/submit/${submit_id}/points`, {
                method: 'POST',
                body: form,
            })

            assigned_points = value;
            saving = false;
            show = false;
        }

    </script>

    <style>
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            background: rgba(255, 255, 255, 0.9);
            width: 100vw;
            height: 100vh;
            z-index: 11;
        }

        .inner {
            width: 50%;
            display: flex;
            align-items: center;
            height: 100%;
            flex-direction: column;
            flex-wrap: wrap;
            align-content: center;
            margin: 0 auto;
            justify-content: center;
        }

        button {
            width: 100%;
        }
    </style>
    
    <div on:contextmenu={ctxMenu} on:keydown={keydown} on:click={click} bind:this={container}>
        {#if submits != 0}
        <a href={link} style="color: {color}">
            { isNaN(parseFloat(assigned_points)) ? '?' : assigned_points}
        </a>
        {/if}
        {#if show}
        <div class="overlay">
            <div class="inner">
                <h2>{login}</h2>
                <h3>{task}</h3>
                <form on:submit|preventDefault={save}>
                    <input class="form-control" type="number" bind:value={value} autofocus>
                    <button class="btn btn-success mt-1">
                        {#if saving}
                            <div class="spinner-border spinner-border-sm" role="status"></div>
                        {/if}
                        Save
                    </button>
                </form>
            </div>
        </div>
        {/if}
    </div>
