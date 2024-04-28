<script>
    import VPMConfig from "../../public/index.json";
    import SourceConfig from "../../../source.json";

    let helpDialogTarget;

    function addURLToVCC() {
        const url = new URL("vcc://vpm/addRepo");
        url.searchParams.append("url", VPMConfig.url);
        open(url);
    }

    function copyURLToClipboard() {
        navigator.clipboard.writeText(VPMConfig.url);
        alert("Copied URL " + VPMConfig.url);
    }
</script>

<header>
    <section>
        <h2>
            <a target="_blank" href={VPMConfig.url}><span title={VPMConfig.id}>{VPMConfig.name}</span></a>
        </h2>
    </section>
    <section id="header">
        <input disabled value={VPMConfig.url} />
        <button on:click={addURLToVCC}>Add to VCC</button>
        <button on:click={copyURLToClipboard}>Copy</button>
        
        <button on:click={() => helpDialogTarget.showModal()}>
            ?
        </button>
        <dialog id="dialog" bind:this={helpDialogTarget}>
            <h3>? How to add this listing to your VCC</h3>
            <span
                >To add this listing to your VCC (VRChat Creator Companion), do
                the following</span
            >
            <ul>
                <li>Open your VCC and go to Settings</li>
                <li>Click the "Packages" tab</li>
                <li>Click "Add Repository"</li>
                <li>
                    In the field that appears - paste the url displayed below
                </li>
                <li>Click "Add"</li>
                <li>Check the repository info and click "I Understand"</li>
                <li>
                    Go to any of your projects to see the packages from the
                    newly added listing.
                </li>
            </ul>
            <h4>Listing URL</h4>
            <div id="dialog-footer">
                <input disabled value={SourceConfig.url} />
                <button on:click={copyURLToClipboard}>Copy</button>
            </div>
            You can read more about Package Listings
            <a href="https://vcc.docs.vrchat.com">on the VCC docs</a>
        </dialog>
    </section>
</header>

<style>
    #header{
        display: flex;
        justify-content: center;
        gap: 1rem;
    }

    #header > input {
        margin: 0;
        flex-grow: 1;
        text-align: center;
    }

    #dialog ul > li {
        text-align: left;
    }

    #dialog-footer{
        display: flex;
        gap: 1rem;
        margin-bottom: 1rem;
    }

    #dialog-footer > input {
        flex-grow: 1;
        margin: 0;
        text-align: center;
    }
</style>