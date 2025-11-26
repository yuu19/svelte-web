<script lang="ts">
        type Step = {
                title: string;
                detail: string;
        };

        let { steps = [] } = $props<{ steps?: Step[] }>();
</script>

<ol class="space-y-3">
        {#each steps as step, index}
                <li class="flex gap-3 rounded-xl border border-white/10 bg-white/5 p-4 text-sm text-white/80">
                        <span class="flex h-8 w-8 items-center justify-center rounded-full bg-sky-400/30 text-sm font-semibold text-sky-50">
                                {index + 1}
                        </span>
                        <div>
                                <p class="text-base font-semibold text-white">{step.title}</p>
                                <p class="text-white/70">{step.detail}</p>
                        </div>
                </li>
        {/each}
</ol>
