# Firechat
Link del ejercicio:
https://angularfirebase.com/lessons/build-group-chat-with-firestore/

NOTA: FALTA LA CREACION DEL COMPONENTE HOME, AHI SE HACE EL INICIO DE SESION Y EL ROUTING HACIA CHAT.
ESTE ES EL CODIGO PARA BORRAR O ARCHIVAR LOS DATOS EN CASO DE QUE EL DOCUMENTO EXCEDA 1MB:
//-----------------------------------------------------
import * as functions from "firebase-functions";
import * as admin from "firebase-admin";
admin.initializeApp();

const db = admin.firestore();

export const archiveChat = functions.firestore
  .document("chats/{chatId}")
  .onUpdate(change => {
    const data = change.after.data();

    const maxLen = 100;
    const msgLen = data.messages.length;
    const charLen = JSON.stringify(data).length;

    const batch = db.batch();

    if (charLen >= 10000 || msgLen >= maxLen) {

      // Always delete at least 1 message
      const deleteCount = msgLen - maxLen <= 0 ? 1 : msgLen - maxLen
      data.messages.splice(0, deleteCount);
 
      const ref = db.collection("chats").doc(change.after.id);

      batch.set(ref, data, { merge: true });

      return batch.commit();
    } else {
      return null;
    }
  });
  //------------------------------------------------------

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 7.0.3.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).
