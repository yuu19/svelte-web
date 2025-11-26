<script lang="ts">
        let { code, label = 'mdsvex + Svelte' } = $props<{ code: string; label?: string }>();
</script>

<div class="rounded-2xl border border-white/10 bg-black/50 p-4 shadow-inner shadow-black/30">
        <div class="mb-3 flex items-center justify-between text-xs uppercase tracking-[0.2em] text-white/60">
                        <span>{label}</span>
                        <span class="rounded-full border border-white/10 bg-white/10 px-2 py-1 text-[10px] text-white/70">Snippet</span>
        </div>
        <pre class="whitespace-pre-wrap rounded-xl bg-black/60 p-4 font-mono text-xs leading-relaxed text-slate-100">{code}</pre>
</div>
