<script lang="ts">
        type Feature = {
                title: string;
                description: string;
                accent?: string;
        };

        let { items = [] } = $props<{ items?: Feature[] }>();
</script>

<div class="grid gap-6 md:grid-cols-3">
        {#each items as item}
                <article class="rounded-2xl border border-white/10 bg-gradient-to-br from-white/5 via-white/0 to-white/5 p-5 shadow-lg shadow-black/10">
                        <p class="text-xs font-semibold uppercase tracking-[0.2em] text-sky-400">{item.accent ?? 'mdsvex'}</p>
                        <h3 class="mt-2 text-lg font-semibold text-white">{item.title}</h3>
                        <p class="mt-2 text-sm leading-relaxed text-white/70">{item.description}</p>
                </article>
        {/each}
</div>
