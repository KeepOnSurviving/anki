<!--
Copyright: Ankitects Pty Ltd and contributors
License: GNU AGPL, version 3 or later; http://www.gnu.org/licenses/agpl.html
-->
<script lang="ts">
    import * as tr from "lib/i18n";
    import SpinBox from "./SpinBox.svelte";
    import type { DeckOptionsState } from "./lib";

    export let state: DeckOptionsState;
    let config = state.currentConfig;
    let defaults = state.defaults;
    let parentLimits = state.parentLimits;

    $: newCardsGreaterThanParent =
        $config.newPerDay > $parentLimits.newCards
            ? tr.deckConfigDailyLimitWillBeCapped({ cards: $parentLimits.newCards })
            : "";

    $: reviewsTooLow =
        Math.min(9999, $config.newPerDay * 10) > $config.reviewsPerDay
            ? tr.deckConfigReviewsTooLow({
                  cards: $config.newPerDay,
                  expected: Math.min(9999, $config.newPerDay * 10),
              })
            : "";
</script>

<h2>{tr.deckConfigDailyLimits()}</h2>

<SpinBox
    label={tr.schedulingNewCardsday()}
    tooltip={tr.deckConfigNewLimitTooltip()}
    min={0}
    warnings={[newCardsGreaterThanParent]}
    defaultValue={defaults.newPerDay}
    bind:value={$config.newPerDay} />

<SpinBox
    label={tr.schedulingMaximumReviewsday()}
    tooltip={tr.deckConfigReviewLimitTooltip()}
    min={0}
    warnings={[reviewsTooLow]}
    defaultValue={defaults.reviewsPerDay}
    bind:value={$config.reviewsPerDay} />
