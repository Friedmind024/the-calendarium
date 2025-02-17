<script lang="ts">
    import { getContext } from "svelte";
    import type { LeapDay } from "src/@types";
    import LeapDayUI from "./LeapDayInstance.svelte";
    import AddNew from "../Utilities/AddNew.svelte";
    import NoExistingItems from "../Utilities/NoExistingItems.svelte";
    import { CreateLeapDayModal } from "src/settings/modals/leapday/leapday";
    import type Calendarium from "src/main";

    import Details from "../Utilities/Details.svelte";

    export let plugin: Calendarium;

    const calendar = getContext("store");

    const { leapDayStore, leapDayDisabled } = calendar;

    const add = (leapday?: LeapDay) => {
        const modal = new CreateLeapDayModal(plugin.app, $calendar, leapday);
        modal.onClose = () => {
            if (!modal.saved) return;
            if (!modal.leapday.interval.length) return;
            if (!modal.leapday.name) return;
            if (modal.editing && leapday != null && leapday.id) {
                leapDayStore.update(leapday.id, { ...modal.leapday });
            } else {
                leapDayStore.add({ ...modal.leapday });
            }
        };
        modal.open();
    };
</script>

<Details
    name={"Leap Days"}
    open={false}
    desc={`${$leapDayStore.length} leap day${
        $leapDayStore.length != 1 ? "s" : ""
    }`}
>
    <AddNew
        on:click={() => add()}
        disabled={$leapDayDisabled}
        label={$leapDayDisabled ? "At least one named month is required" : ""}
    />

    {#if !$leapDayStore.length}
        <NoExistingItems message={"Create a new leap day to see it here."} />
    {:else}
        <div class="existing-items">
            {#each $leapDayStore as leapday}
                <LeapDayUI
                    {leapday}
                    on:edit={() => add(leapday)}
                    on:delete={() => leapDayStore.delete(leapday.id ?? "")}
                />
            {/each}
        </div>
    {/if}
</Details>

<style>
</style>
