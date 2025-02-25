<script lang="ts">
    import type { Item } from "$lib/types";
    import { redirect } from "@sveltejs/kit";
    let { data } = $props();

    let selectedItemId = $state(data.availableItems.at(0)?.id || 0);
    let cart: Item[] = $state([]);
    let count = $state(1)

    const addToCart = () => {
        let count = $state(1);
        const selectedItem = data.availableItems.find((item) => item.id === selectedItemId);
        if (!selectedItem) { return; }
        if (cart.find((item) => item.id === selectedItem.id)) { return;}

        cart.push(selectedItem);
    }

</script>
<div class="h-screen w-screen flex justify-center items-center">
    <main class="grid grid-cols-2 p-6 gap-6 max-w-[1024px] w-full">
        <section class="w-full">
            <div class="join w-full">
                <select bind:value={selectedItemId} class="select select-bordered w-full join-item">
                    {#each data.availableItems as item}
                        <option value={item.id} disabled={cart.find((v) => v.id === item.id) !== undefined}>{item.name}</option>
                    {/each}
                </select>
                
                <button onclick={addToCart} class="btn btn-primary join-item">Добавить в заказ</button>
            </div>
        </section>
        <select name="color" id="color-select" class="select select-warning w-full max-w-xs">
            <option disabled selected>-- Выбери контейнер--</option>
            <option value="blue">Синий</option>
            <option value="red">Красный</option>
            <option value="green">Зеленый</option>
        </select>
        <form action="?/order" method="post" class="flex flex-col gap-6 h-96 justify-between">
            {#if cart.length > 0}
                <table class="table table-zebra">
                    <tbody>
                        {#each cart as item, id}
                        <tr>
                            <td>
                                <input type="hidden" name="item" value={item.id}>
                                <p>{item.name}</p>
                            </td>
                            <td>
                                <select name="numbers" id="number-select" class="select select-primary w-full max-w-xs">
                                    <option disabled selected>-- Выбери колличество--</option>
                                    <option value="1">1</option>
                                    <option value="2">2</option>
                                    <option value="3">3</option>
                                    <option value="4">4</option>
                                    <option value="5">5</option>
                                    <option value="6">6</option>
                                    <option value="7">7</option>
                                    <option value="8">8</option>
                                    <option value="9">9</option>
                                    <option value="10">10</option>
                                  </select>
                            </td>
                            <td>
                                <button onclick={() => cart = cart.filter((v) => v.id !== item.id)}>Удалить</button>
                            </td>
                            </tr>
                        {/each}
                    </tbody>
                </table>
            {:else}
                <p class="text-center">Выберите предметы для доставки</p>
            {/if}
            <button  disabled={cart.length <= 0} name="Prop"  class="btn btn-accent">
                Заказать
            </button>
        </form>
    </main>
</div>
