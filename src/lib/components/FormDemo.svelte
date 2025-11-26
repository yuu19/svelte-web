<script lang="ts">
        type Field = 'name' | 'email' | 'message';

        const emptyState = {
                name: '',
                email: '',
                message: ''
        } satisfies Record<Field, string>;

        let form = $state({ ...emptyState });
        let touched = $state<Record<Field, boolean>>({ name: false, email: false, message: false });
        let submitted = $state(false);

        const validators: Record<Field, (value: string) => string | null> = {
                name: (value) => (value.trim().length < 2 ? '2文字以上で入力してください' : null),
                email: (value) => (/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(value) ? null : 'メールアドレスの形式が正しくありません'),
                message: (value) => (value.trim().length < 10 ? '10文字以上で入力してください' : null)
        };

        const labels: Record<Field, string> = {
                name: 'お名前',
                email: 'メール',
                message: 'メッセージ'
        };

        const errors = $derived(
                Object.fromEntries(
                        (Object.entries(form) as [Field, string][]).map(([field, value]) => [field, validators[field](value)])
                ) as Record<Field, string | null>
        );

        function update(field: Field, value: string) {
                form = { ...form, [field]: value };
                touched[field] = true;
                submitted = false;
        }

        function handleSubmit(event: Event) {
                event.preventDefault();
                touched = { name: true, email: true, message: true };
                const hasError = Object.values(errors).some(Boolean);
                submitted = !hasError;
                if (!hasError) {
                        form = { ...emptyState };
                }
        }
</script>

<form class="space-y-4" onsubmit={handleSubmit}>
        <div class="grid gap-4 md:grid-cols-2">
                {#each (['name', 'email'] satisfies Field[]) as field}
                        <label class="group block rounded-xl border border-white/10 bg-white/5 p-4 text-sm text-white/80 shadow-inner shadow-black/20">
                                <span class="flex items-center justify-between text-xs font-semibold uppercase tracking-[0.2em] text-white/60">
                                        {labels[field]}
                                        {#if touched[field] && !errors[field]}
                                                <span class="text-emerald-300">OK</span>
                                        {/if}
                                </span>
                                <input
                                        class="mt-1 w-full rounded-lg border border-white/10 bg-black/30 px-3 py-2 text-white outline-none ring-0 transition focus:border-sky-300 focus:bg-black/20"
                                        name={field}
                                        autocomplete={field === 'email' ? 'email' : 'name'}
                                        value={form[field]}
                                        oninput={(event) => update(field, event.currentTarget.value)}
                                />
                                {#if touched[field] && errors[field]}
                                        <p class="mt-1 text-xs text-amber-300">{errors[field]}</p>
                                {/if}
                        </label>
                {/each}
        </div>

        <label class="block rounded-xl border border-white/10 bg-white/5 p-4 text-sm text-white/80 shadow-inner shadow-black/20">
                <span class="text-xs font-semibold uppercase tracking-[0.2em] text-white/60">{labels.message}</span>
                <textarea
                        class="mt-2 h-28 w-full rounded-lg border border-white/10 bg-black/30 px-3 py-2 text-white outline-none transition focus:border-sky-300 focus:bg-black/20"
                        name="message"
                        value={form.message}
                        oninput={(event) => update('message', event.currentTarget.value)}
                ></textarea>
                {#if touched.message && errors.message}
                        <p class="mt-1 text-xs text-amber-300">{errors.message}</p>
                {/if}
        </label>

        <div class="flex flex-wrap items-center gap-3">
                <button
                        type="submit"
                        class="rounded-lg bg-gradient-to-r from-sky-400 via-fuchsia-400 to-amber-300 px-4 py-2 text-sm font-semibold text-slate-900 shadow-lg shadow-sky-400/30 transition hover:brightness-105"
                >
                        バリデーションして送信
                </button>
                {#if submitted}
                        <span class="rounded-full border border-emerald-300/40 bg-emerald-400/20 px-3 py-1 text-xs font-semibold text-emerald-50">
                                送信完了！ サーバーに渡す直前の感覚を体験できます。
                        </span>
                {:else if Object.values(errors).some(Boolean) && Object.values(touched).some(Boolean)}
                        <span class="rounded-full border border-amber-300/40 bg-amber-400/10 px-3 py-1 text-xs font-semibold text-amber-50">
                                まだ足りない項目があります。
                        </span>
                {/if}
        </div>
</form>
