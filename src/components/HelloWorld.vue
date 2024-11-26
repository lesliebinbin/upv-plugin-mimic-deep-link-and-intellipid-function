<script setup lang="ts">
import { Application } from '@caxperts/universal.api';
import { CaxApiCommand } from '@caxperts/universal.api/Internal/CaxApiCommand';
import { Api } from '@caxperts/universal.api/Internal/APIConnector';
import { ApiCommands } from '@caxperts/universal.api/Util/Enums';
async function openLink() {

  try{
    const scene = (await Application.getInstance().Scenes3d.get())[0];
    const filter =  scene.DefaultFilter;
    filter.Condition = 'Name=D-240';
    await filter.select();
    await filter.fit();
    
    // eslint-disable-next-line @typescript-eslint/ban-ts-comment
    // @ts-ignore:next-line
    const targetCommand = new CaxApiCommand('Target');
    targetCommand.commandParameters.push('IntelliPid');
    await Api.get().sendCommandWithReturnType(targetCommand);
    await Api.get().sendCommand(new CaxApiCommand(ApiCommands.OpenIntelliPidDrawings));
    await Api.get().sendCommand(new CaxApiCommand(ApiCommands.Select));
    await Api.get().sendCommand(new CaxApiCommand(ApiCommands.Fit));
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
