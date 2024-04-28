<script>
    import VPMConfig from "../../public/index.json";

    const latestPackages = Object.entries(VPMConfig.packages).map(([vpmPackageId, vpmPackage]) => {
        const latest = vpmPackage.versions[Object.keys(vpmPackage.versions)[0]];
        latest.otherVersions = vpmPackage.versions
        return [vpmPackageId, latest]
    })

    const dialogPackages = []
</script>

<main>
    <div id="package-list">
        <div>Package Name</div>
        <div>Latest version</div>
        <div>More details</div>
        {#each latestPackages as [vpmPackageId, vpmPackage]}
            <div>
                <h3>
                    {vpmPackage.displayName}
                    {#if vpmPackage.author?.name}
                        <br/>By
                        {#if vpmPackage.author?.url}
                            <a target="_blank" href="{vpmPackage.author.url}">{vpmPackage.author.name}</a>
                        {:else}
                            {vpmPackage.author.name}
                        {/if}
                        {#if vpmPackage.author?.email}
                            (<a href="mailto:{vpmPackage.author.email}">{vpmPackage.author.email}</a>)
                        {/if}
                    {/if}
                </h3>
                <div>{vpmPackage.description}</div>
                <div><h4>{vpmPackageId}</h4></div>
                {#if vpmPackage.keywords?.length}
                <div>Tags: {vpmPackage.keywords.join(' | ')}
                </div>
                {/if}
            </div>
            <div>
                <h4>{vpmPackage.version}</h4>
            </div>
            <div>
                <button on:click={dialogPackages[vpmPackageId].showModal()}>
                    i
                </button>
                <dialog bind:this={dialogPackages[vpmPackageId]}>
                    <h3>{vpmPackage.displayName}</h3>
                    <p>
                        {vpmPackage.description}
                    </p>
                    <p>
                        Versions:
                        <select>
                            {#each Object.keys(vpmPackage.otherVersions) as version}
                                <option>{version}</option>
                            {/each}
                        </select>        
                    </p>
                    <p>Unity minimal version: {vpmPackage.unity || "NA"}</p>
                    <p>
                        Changelog: 
                        {#if vpmPackage.changelogUrl}
                            <a target="_blank" href="{vpmPackage.changelogUrl}">Here</a>
                        {:else}
                            NA
                        {/if}
                    </p>
                    <p>
                        Documentation: 
                        {#if vpmPackage.changelogUrl}
                            <a target="_blank" href="{vpmPackage.documentationUrl}">Here</a>
                        {:else}
                            NA
                        {/if}
                    </p>
                    <p>
                        Unity Dependencies: 
                        {#if Object.keys(vpmPackage?.dependencies || {}).length}
                            <ul>
                            {#each Object.entries(vpmPackage.dependencies) as [depKey, depValue]}
                                <li>{depKey} {depValue}</li>
                            {/each}
                            </ul>
                        {:else}
                            NA
                        {/if}
                    </p>
                    <p>
                        VPM Dependencies: 
                        {#if Object.keys(vpmPackage?.vpmDependencies || {}).length}
                        <ul>
                            {#each Object.entries(vpmPackage.vpmDependencies) as [depKey, depValue]}
                                <li>{depKey} {depValue}</li>
                            {/each}
                            </ul>
                        {:else}
                            NA
                        {/if}
                    </p>
                    <p>Tags: 
                        {#if vpmPackage.keywords}
                            {vpmPackage.keywords.join(' | ')}
                        {:else}
                            NA
                        {/if}
                    </p>
                    <p>
                        Licence: 
                        {#if vpmPackage.licensesUrl && vpmPackage.license}
                            <a target="_blank" href="{vpmPackage.licensesUrl}">{vpmPackage.license}</a>
                        {:else}
                            {vpmPackage.license || "NA"}
                        {/if}
                    </p>    
                </dialog>
            </div>
        {/each}
    </div>
</main>

<style>
    #package-list {
        display: grid;
        grid-template-columns: 1fr auto auto;
    }

    #package-list > div {
        display: flex;
        flex-direction: column;
        justify-content: center;
        border: 2px solid black;
        padding: 5px;
    }

    #package-list > div:not(:nth-child(3n + 1)) {
        align-items: center;
    }
</style>