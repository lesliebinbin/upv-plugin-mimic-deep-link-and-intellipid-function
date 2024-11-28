<script setup lang="ts">
import { Application } from '@caxperts/universal.api';
import { CaxApiCommand } from '@caxperts/universal.api/Internal/CaxApiCommand';
import { Api } from '@caxperts/universal.api/Internal/APIConnector';
import { ApiCommands } from '@caxperts/universal.api/Util/Enums';
async function openLink() {

  try{
    const runtimeInUPV = Application.getInstance().available();
    console.log(runtimeInUPV);
    const scene = (await Application.getInstance().ScenesPid.get())[0];
    const filter =  scene.DefaultFilter;
    filter.Condition = 'Equipment ITEMTAG=D-240';
    const command = filter.createCommand(ApiCommands.OpenIntelliPidDrawings);
    await Api.get().sendCommand(command);
    await filter.select();
    await filter.fit();
  }
  catch(err){
    Application.getInstance().showMessage(err as string);
  }
}

</script>

<template>
  <button :onclick="openLink">
    Open An Example UPV Deep Link
  </button>
</template>

<style scoped>
a {
  color: #42b983;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
}

code {
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}
</style>
upvapi://http://demo.universalplantviewer.com/demoPlant/6/0?Name=D-240&CMD!Target=IntelliPid&CMD!OpenIntellipidDrawings&CMD!Select&CMD!Fit
