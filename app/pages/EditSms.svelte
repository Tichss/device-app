<script lang="ts">
    import type { Device } from "~/components/Device.svelte";
    import type { Sms } from "~/components/Sms.svelte";
    import { setString } from "@nativescript/core/application-settings";
    import { goBack } from "svelte-native";
    import { createEventDispatcher, onMount } from "svelte";
    import { devices } from "~/store";
    export let parentDevice: Device;
    export let sms: Sms;
    export let isNew: boolean;

    const dispatch = createEventDispatcher();

    onMount(() => {
        console.log("asd");
    });

    function onDelete() {
        console.log("onDelete");
    }
    function onSave() {
        if(isNew) {
            parentDevice.smses.push(sms);
            parentDevice = parentDevice;
            setString("devices", JSON.stringify($devices));
            
        } else {
            let i = parentDevice.smses.findIndex(smsTemp => {smsTemp.name == sms?.name;})
            parentDevice.smses[i] = sms;
            console.log('on:save modified sms');
        }
        $devices = $devices;
        dispatch('save', parentDevice);
        goBack();
    }
</script>

<page>
    <actionBar title={`Eszköz: ${parentDevice.name} - Új sms`}>
        {#if !isNew}
            <actionItem
                ios.systemIcon="16"
                ios.position="right"
                text="Törlés"
                android.position="popup"
            />
        {/if}
    </actionBar>
    <scrollView>
        <stackLayout>
            <label text="Sms neve*" />
            <textField bind:text={sms.name} />
            <label text="Sms tartalma*" />
            <textView bind:text={sms.text} />
            <button text="Mentés" on:tap={onSave} />
            <button text="Törlés" on:tap={onDelete} />
        </stackLayout>
    </scrollView>
</page>

<style lang="scss">
    stackLayout {
        padding: 30px;

        label {
            font-size: 16px;
            color: gray;
        }

        textField,
        textView {
            font-size: 18px;
            margin: 0;
        }
        textView {
            border-width: 1;
            border-color: gray;
            border-radius: 5;
        }

        button {
            margin: 0;
        }
    }
</style>
