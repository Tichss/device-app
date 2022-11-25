<script lang="ts">
    import { navigate } from "svelte-native";
    import Home from "./Home.svelte";
    import { devices } from "../store";
    import { setString } from "@nativescript/core/application-settings";
    import { goBack } from "svelte-native";
    import type { Device } from "~/components/Device.svelte";

    export let device: Device = { name: "", phone: "", smses: [] };
    export let isNew = false;

    function onSave() {
        if (isNew) {
            $devices.push(device);
            $devices = $devices;
            setString("devices", JSON.stringify($devices));
        }
        goBack();
    }

    function onDelete() {
        $devices = $devices.filter((dev) => dev.name !== device.name);
        setString("devices", JSON.stringify($devices));
        goBack();
    }

    function onAddSms() {
        device.smses.push("");
        device = device;
    }
</script>

<page>
    <actionBar title={isNew ? "Új eszköz" : `Eszköz: ${device.name}`}>
        {#if !isNew}
            <actionItem
                ios.systemIcon="16"
                ios.position="right"
                text="Törlés"
                android.position="popup"
            />
        {/if}
    </actionBar>
    <stackLayout>
        <label text="Eszközneve*" />
        <textField bind:text={device.name} />
        <label text="Eszköz száma*" />
        <textField bind:text={device.phone} keyboardType="number" />
        {#if device.smses?.length > 0}
            <label text="Eszköz sms listája" />
            {#each device.smses as sms}
                <textField bind:text={sms} />
            {/each}
        {/if}
        <button text="Sms hozzáadása" on:tap={onAddSms} />
        <button text="Mentés" on:tap={onSave} />
        {#if !isNew}
            <button text="Törlés" on:tap={onDelete} />
        {/if}
    </stackLayout>
</page>

<style lang="scss">
    /* your styles go here */
</style>
