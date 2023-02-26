<script lang="ts">
    import * as appSettings from "@nativescript/core/application-settings";
    import { onMount } from "svelte";
    import { navigate } from "svelte-native";
    import EditDevice from "./EditDevice.svelte";
    import Device from "../components/Device.svelte";
    import { devices } from "../store";
    import { requestPermission } from "nativescript-permissions";
    import { setString } from "@nativescript/core/application-settings";

    onMount(() => {
        $devices = JSON.parse(appSettings.getString("devices", "[]"));

        requestPermission(
            android.Manifest.permission.CALL_PHONE,
            "I need these permissions because I'm cool"
        )
            .then(() => {
                console.log("Woo Hoo, I have the power!");
            })
            .catch(() => {
                console.log("Uh oh, no permissions - plan B time!");
            });
    });

    function onNewDevice() {
        navigate({
            page: EditDevice,
            props: { device: { name: "", phone: "", smses: [] }, isNew: true },
        });
    }
</script>

<page>
    <actionBar title="Eszközök">
        <actionItem
            ios.systemIcon="16"
            ios.position="right"
            text="Eszköz hozzáadása"
            android.position="popup"
            on:tap={onNewDevice}
        />
    </actionBar>

    <scrollView>
        <stackLayout>
            <flexboxLayout flexWrap="wrap">
                <button text="add" on:tap={onNewDevice} />
                {#each $devices as device, i}
                    <flexboxLayout>
                        <Device bind:device={device} {i} />
                    </flexboxLayout>
                {/each}
            </flexboxLayout>
        </stackLayout>
    </scrollView>
</page>

<style lang="scss">
    stackLayout {
        border-width: 3;
        border-color: orangered;
        border-radius: 20;
        padding: 10;

        flexboxLayout {
            button {
                width: 25%;
                min-height: 80;
                margin: 0;
            }

            flexboxLayout {
                /* border-width: 1;
                border-color: black; */
                width: 25%;
                min-height: 80;
                align-items: center;
                justify-content: center;
                /* background-color: rgb(60, 255, 0); */

                .t1 {
                    background-color: rgb(255, 0, 98);
                }
            }
        }
    }
</style>
