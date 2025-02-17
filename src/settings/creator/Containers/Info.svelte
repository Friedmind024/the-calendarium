<script lang="ts">
    import { getContext } from "svelte";
    import TextAreaComponent from "../Settings/TextAreaComponent.svelte";
    import TextComponent from "../Settings/TextComponent.svelte";
    import ToggleComponent from "../Settings/ToggleComponent.svelte";
    import Details from "../Utilities/Details.svelte";
    import { DEFAULT_CALENDAR } from "src/settings/settings.constants";
    import { DEFAULT_FORMAT } from "src/utils/constants";

    const calendar = getContext("store");

    $: displayDayNumber = $calendar.static.displayDayNumber;
    $: incrementDay = $calendar.static.incrementDay;

    $: validName = $calendar.name != null && $calendar.name.length;

    if (!$calendar.inlineEventTag)
        $calendar.inlineEventTag = DEFAULT_CALENDAR.inlineEventTag;

    const descFormat = () =>
        createFragment((e) => {
            e.createSpan({
                text: "Event dates will be parsed using this format.",
            });
            e.createEl("br");
            e.createSpan({
                text: "Information on how the format works can be seen ",
            });
            e.createEl("a", {
                href: "https://plugins.javalent.com/calendarium/create-calendar#Date+Format",
                text: "here",
            });
            e.createSpan({
                text: ".",
            });
        });
</script>

<Details
    name={"Basic Info"}
    warn={!validName}
    label={"The calendar must have a name"}
>
    <div class="calendarium-info">
        {#key $calendar.name}
            <TextComponent
                name={"Calendar Name"}
                warn={!validName}
                desc={!validName ? "The calendar must have a name" : ""}
                value={$calendar.name}
                on:blur={(evt) => {
                    if (evt.detail === $calendar.name) return;
                    $calendar.name = evt.detail;
                }}
            />
        {/key}
        <!-- on:change={(evt) => ($calendar.name = evt.detail)} -->
        <TextAreaComponent
            name={"Calendar Description"}
            value={$calendar.description ?? ""}
            on:blur={(evt) => {
                if (evt.detail === $calendar.description) return;
                $calendar.description = evt.detail;
            }}
        />
        <ToggleComponent
            name={"Display Day Number"}
            desc={"Display day of year in Day View"}
            value={displayDayNumber}
            on:click={() => {
                $calendar.static.displayDayNumber =
                    !$calendar.static.displayDayNumber;
            }}
        />
        <TextComponent
            name={"Date Format"}
            desc={descFormat()}
            value={$calendar.dateFormat ?? DEFAULT_FORMAT}
            on:blur={(evt) => {
                if (evt.detail === $calendar.dateFormat) return;
                $calendar.dateFormat = evt.detail;
            }}
        />
        <!-- <ToggleComponent
            name={"Auto Increment Day"}
            desc={"Automatically increment the current day every real-world day."}
            value={incrementDay}
            on:click={() => {
                $calendar.static.incrementDay = !$calendar.static.incrementDay;
            }}
        /> -->
    </div>
</Details>

<style>
    .calendarium-info :global(.setting-item) {
        padding-top: 18px;
    }
    .calendarium-info :global(.calendarium-description) {
        display: flex;
        flex-flow: column;
        align-items: flex-start;
    }
    .calendarium-info :global(.calendarium-description) :global(textarea) {
        width: 100%;
    }
</style>
