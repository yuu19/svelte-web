<script lang="ts">
        let { title, description, eyebrow = 'Live demo', children } = $props<{
                title: string;
                description: string;
                eyebrow?: string;
                children?: () => unknown;
        }>();
</script>

<section class="flex flex-col gap-4 rounded-2xl border border-white/10 bg-white/5 p-6 shadow-lg shadow-black/10 backdrop-blur">
        <div class="flex items-center justify-between gap-3">
                <div>
                        <p class="text-xs font-semibold uppercase tracking-[0.2em] text-sky-300">{eyebrow}</p>
                        <h2 class="text-xl font-semibold text-white">{title}</h2>
                        <p class="mt-1 text-sm text-white/70">{description}</p>
                </div>
                <div class="hidden shrink-0 rounded-full bg-gradient-to-br from-sky-400/20 via-fuchsia-400/20 to-amber-300/20 p-3 text-sky-200 md:block">
                        <span aria-hidden="true">★</span>
                </div>
        </div>
        <div class="rounded-xl border border-white/10 bg-black/20 p-4">
                {@render children?.()}
        </div>
</section>
