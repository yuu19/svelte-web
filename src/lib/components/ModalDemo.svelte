<script lang="ts">
        let { title = 'モーダルダイアログ', description = 'mdsvex の記事に組み込める、クリックで開閉するモーダル UI のサンプルです。' } =
                $props<{
                        title?: string;
                        description?: string;
                }>();

        let open = $state(false);
        let confirmCount = $state(0);
        let dialogRef = $state<HTMLDivElement | null>(null);

        const stateLabel = $derived(open ? '開いています' : '閉じています');
        const instanceId = crypto.randomUUID();
        const titleId = `modal-title-${instanceId}`;
        const descriptionId = `modal-description-${instanceId}`;

        $effect(() => {
                if (open) {
                        dialogRef?.focus();
                }
        });

        function openModal() {
                open = true;
        }

        function closeModal() {
                open = false;
        }

        function handleBackdropClick() {
                closeModal();
        }

        function handleBackdropKeydown(event: KeyboardEvent) {
                if (event.key === 'Enter' || event.key === ' ') {
                        event.preventDefault();
                        closeModal();
                }
        }

        function handleKeydown(event: KeyboardEvent) {
                if (event.key === 'Escape') {
                        closeModal();
                }
        }

        function confirmAction() {
                confirmCount += 1;
                closeModal();
        }
</script>

<section class="space-y-4 text-white">
        <header class="flex flex-col gap-2">
                <p class="text-xs font-semibold uppercase tracking-[0.25em] text-sky-300">Modal Demo</p>
                <div class="flex flex-wrap items-center gap-3">
                        <h2 class="text-2xl font-semibold">{title}</h2>
                        <span class="rounded-full border border-white/10 bg-white/5 px-3 py-1 text-xs text-white/70">
                                状態: {stateLabel}
                        </span>
                </div>
                <p class="max-w-2xl text-sm text-white/70">{description}</p>
        </header>

        <div class="flex flex-wrap gap-3">
                <button
                        class="rounded-lg bg-gradient-to-r from-sky-400 via-fuchsia-400 to-amber-300 px-4 py-2 text-sm font-semibold text-slate-900 shadow-lg shadow-sky-400/30 transition hover:brightness-105"
                        onclick={openModal}
                        aria-expanded={open}
                        aria-controls={open ? titleId : undefined}
                >
                        モーダルを開く
                </button>
                <button
                        class="rounded-lg border border-white/15 bg-white/5 px-4 py-2 text-sm font-semibold text-white/80 transition hover:border-sky-300 hover:text-white"
                        onclick={closeModal}
                        disabled={!open}
                >
                        閉じる
                </button>
                <div class="flex items-center gap-2 rounded-lg border border-white/10 bg-white/5 px-3 py-2 text-xs text-white/70">
                        <span>確認済み:</span>
                        <span class="rounded-full bg-emerald-400/20 px-2 py-0.5 text-emerald-100">{confirmCount} 回</span>
                </div>
        </div>

        {#if open}
                <div
                        class="fixed inset-0 z-20 flex items-center justify-center px-4"
                        role="button"
                        tabindex="0"
                        aria-label="モーダルを閉じる"
                        onclick={handleBackdropClick}
                        onkeydown={handleBackdropKeydown}
                >
                        <div class="absolute inset-0 bg-black/60 backdrop-blur-sm"></div>
                        <div
                                role="dialog"
                                aria-modal="true"
                                aria-labelledby={titleId}
                                aria-describedby={descriptionId}
                                tabindex="-1"
                                class="relative z-10 w-full max-w-lg rounded-2xl border border-white/15 bg-gradient-to-br from-slate-900/95 via-slate-900/90 to-slate-900/95 p-6 shadow-2xl shadow-black/50"
                                onclick={(event) => event.stopPropagation()}
                                onkeydown={handleKeydown}
                                bind:this={dialogRef}
                        >
                                <div class="flex items-start justify-between gap-4">
                                        <div>
                                                <p class="text-xs font-semibold uppercase tracking-[0.25em] text-sky-300">Live modal</p>
                                                <h3 id={titleId} class="text-xl font-semibold">Svelte 5 で作るモーダル</h3>
                                                <p id={descriptionId} class="mt-1 text-sm text-white/70">バックドロップクリックや ESC キーで閉じられます。</p>
                                        </div>
                                        <button
                                                class="rounded-full border border-white/10 bg-white/5 p-2 text-white/80 transition hover:border-sky-300 hover:text-white"
                                                type="button"
                                                aria-label="閉じる"
                                                onclick={closeModal}
                                        >
                                                ×
                                        </button>
                                </div>

                                <div class="mt-4 space-y-3 text-sm text-white/80">
                                        <p>
                                                モーダル内にも自由にコンテンツを差し込めます。mdsvex なら記事中にこのデモを挿入し、読者はその場
                                                で挙動を試せます。
                                        </p>
                                        <ul class="list-disc space-y-1 pl-5 text-white/70">
                                                <li>クリックで開く / バックドロップで閉じる</li>
                                                <li>ESC キーでクローズ</li>
                                                <li>カウンター更新で簡単な状態管理</li>
                                        </ul>
                                </div>

                                <div class="mt-6 flex flex-wrap gap-3">
                                        <button
                                                class="rounded-lg bg-emerald-400 px-4 py-2 text-sm font-semibold text-slate-900 shadow-lg shadow-emerald-400/30 transition hover:brightness-105"
                                                type="button"
                                                onclick={confirmAction}
                                        >
                                                これで進める
                                        </button>
                                        <button
                                                class="rounded-lg border border-white/15 bg-white/5 px-4 py-2 text-sm font-semibold text-white/80 transition hover:border-sky-300 hover:text-white"
                                                type="button"
                                                onclick={closeModal}
                                        >
                                                キャンセル
                                        </button>
                                </div>
                        </div>
                </div>
        {/if}
</section>
