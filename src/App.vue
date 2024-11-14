<template>
  <ejs-message id="msg" :content="messageState.content" :severity="messageState.severity" v-if="messageState.showMessage"></ejs-message>
  <ejs-schedule height='550px' width='100%' :locale='locale' :selectedDate='selectedDate' :eventSettings='eventSettings' @actionComplete="onActionComplete">
    <e-views>
      <e-view option='Day'></e-view>
      <e-view option='Week'></e-view>
      <e-view option='Month'></e-view>
    </e-views>
    <e-resources>
      <e-resource field='CategoryId' title='Kategória' name='Categories' :dataSource='categoryResourceDataSource' :allowMultiple='true' textField='Text' idField='Id' colorField='Color'>
      </e-resource>
    </e-resources>
  </ejs-schedule>
</template>

<script setup>
//Lokalizáció

import { loadCldr,L10n} from '@syncfusion/ej2-base';
import huNumberData from '@syncfusion/ej2-cldr-data/main/hu/numbers.json';
import hutimeZoneData from '@syncfusion/ej2-cldr-data/main/hu/timeZoneNames.json';
import huGregorian from '@syncfusion/ej2-cldr-data/main/hu/ca-gregorian.json';
import huNumberingSystem from '@syncfusion/ej2-cldr-data/supplemental/numberingSystems.json';

loadCldr(huNumberData, hutimeZoneData, huGregorian, huNumberingSystem);

L10n.load({
    "hu": {
    "schedule": {
        "day": "Nap",
        "week": "Hét",
        "workWeek": "Munka hét",
        "month": "Hónap",
        "year": "Év",
        "agenda": "Napló",
        "weekAgenda": "Hét Naplója",
        "workWeekAgenda": "Munka hét Naplója",
        "monthAgenda": "Hónap Naplója",
        "today": "Ma",
        "noEvents": "Nincsenek bejegyzések",
        "emptyContainer": "Nincs munkaidő ütemezve ezen a napon.",
        "allDay": "Egész nap",
        "start": "Kezdés",
        "end": "Befejezés",
        "more": "több",
        "close": "Bezárás",
        "cancel": "Mégse",
        "noTitle": "(Nincs cím)",
        "delete": "Törlés",
        "deleteEvent": "Munkaidő bejegyzés törlése",
        "deleteMultipleEvent": "Több bejegyzés törlése",
        "selectedItems": "Kiválasztott elemek",
        "deleteSeries": "Sorozat törlése",
        "edit": "Szerkesztés",
        "editSeries": "Sorozat szerkesztése",
        "editEvent": "Munkaidő bejegyzés szerkesztése",
        "createEvent": "Létrehozás",
        "subject": "Téma",
        "addTitle": "Cím hozzáadása",
        "moreDetails": "További részletek",
        "moreEvents": "Több bejegyzés",
        "save": "Mentés",
        "editContent": "Csak ezt a bejegyzést szeretnéd szerkeszteni, vagy az egész sorozatot?",
        "deleteRecurrenceContent": "Csak ezt a bejegyzést szeretnéd törölni, vagy az egész sorozatot?",
        "deleteContent": "Biztosan törölni szeretnéd ezt az bejegyzést?",
        "deleteMultipleContent": "Biztosan törölni szeretnéd a kiválasztott bejegyzéseket?",
        "newEvent": "Új Munkaidő bejegyzés",
        "title": "Cím",
        "location": "Helyszín",
        "description": "Leírás",
        "timezone": "Időzóna",
        "startTimezone": "Kezdő időzóna",
        "endTimezone": "Befejező időzóna",
        "repeat": "Ismétlés",
        "saveButton": "Mentés",
        "cancelButton": "Mégse",
        "deleteButton": "Törlés",
        "recurrence": "Ismétlődés",
        "wrongPattern": "Az ismétlési minta érvénytelen.",
        "seriesChangeAlert": "A sorozat egyes példányain végrehajtott módosítások törlésre kerülnek, és azok a bejegyzések újra a sorozatnak megfelelően fognak megjelenni.",
        "createError": "Az esemény időtartama rövidebb kell legyen, mint az ismétlődés gyakorisága. Rövidítsd az időtartamot, vagy módosítsd az ismétlés mintát az ismétlődő bejegyzés szerkesztőjében.",
        "recurrenceDateValidation": "Néhány hónapban kevesebb nap van, mint a választott dátum. Ezekben a hónapokban a bejegyzés az utolsó napon fog megtörténni.",
        "sameDayAlert": "Két példány ugyanazon bejegyzésből nem történhet meg ugyanazon a napon.",
        "occurenceAlert": "Nem lehet újra ütemezni egy esemény előfordulását, ha az átugrik egy későbbi előfordulás fölött.",
        "editRecurrence": "Ismétlés szerkesztése",
        "repeats": "Ismétlődik",
        "alert": "Figyelmeztetés",
        "startEndError": "A kiválasztott befejező dátum előbb van, mint a kezdő dátum.",
        "invalidDateError": "A megadott dátum érvénytelen.",
        "blockAlert": "Bejegyzések nem ütemezhetők a letiltott időszakokba.",
        "ok": "Ok",
        "yes": "Igen",
        "no": "Nem",
        "occurrence": "Előfordulás",
        "series": "Sorozat",
        "previous": "Előző",
        "next": "Következő",
        "timelineDay": "Idővonal Nap",
        "timelineWeek": "Idővonal Hét",
        "timelineWorkWeek": "Idővonal Munka hét",
        "timelineMonth": "Idővonal Hónap",
        "expandAllDaySection": "Bővítés",
        "collapseAllDaySection": "Összecsukás",
        "timelineYear": "Idővonal Év",
        "editFollowingEvent": "Következő események",
        "deleteTitle": "Esemény törlése",
        "editTitle": "Esemény szerkesztése",
        "beginFrom": "Kezdjük innen",
        "endAt": "Befejezés itt",
        "searchTimezone": "Időzóna keresése",
        "noRecords": "Nincsenek feljegyzések",
        "of": "ból/ből"
    },
    "recurrenceeditor": {
        "none": "Nincs",
        "daily": "Naponta",
        "weekly": "Hetente",
        "monthly": "Havonta",
        "month": "Hónap",
        "yearly": "Évente",
        "never": "Mindíg",
        "until": "Vége",
        "count": "Megadott alkalommal",
        "first": "Első",
        "second": "Második",
        "third": "Harmadik",
        "fourth": "Negyedik",
        "last": "Utolsó",
        "repeat": "Ismétlés",
        "repeatEvery": "Ismétlés minden",
        "on": "Ismétlés ekkor",
        "end": "Időtartam",
        "onDay": "Nap",
        "days": "Nap(on)",
        "weeks": "Hét(en)",
        "months": "Hónap(ban)",
        "years": "Év(ben)",
        "every": "minden",
        "summaryTimes": "alkalom",
        "summaryOn": "ezen a napon",
        "summaryUntil": "amíg",
        "summaryRepeat": "Ismétlődik",
        "summaryDay": "nap(on)",
        "summaryWeek": "hét(en)",
        "summaryMonth": "hónap(ban)",
        "summaryYear": "év(ben)"
    }
}
});

import { provide,reactive,watch } from "vue";
import {
  ScheduleComponent as EjsSchedule, ViewsDirective as EViews, ViewDirective as EView,
  ResourcesDirective as EResources, ResourceDirective as EResource,
  Day,Week, Month, 
} from "@syncfusion/ej2-vue-schedule";
import { MessageComponent as EjsMessage } from "@syncfusion/ej2-vue-notifications";

const categoryResourceDataSource = [
    { Text: 'Projekt', Id: 1, Color: '#df5286' },
    { Text: 'Ügyfél', Id: 2, Color: '#7fa900' }
];

// Ellenőrzi, hogy van-e mentett adat a localStorage-ban, és betölti azt
const savedEvents = JSON.parse(localStorage.getItem("scheduleEvents"));
const scheduleData = savedEvents;
const locale = 'hu';

// Reaktív objektum az üzenethez
const messageState = reactive({
  showMessage: false
});

// Események beállítása (reaktívvá teszem)
const eventSettings = reactive({
  dataSource: scheduleData || [],
});
const selectedDate = new Date();
provide("schedule", [Day, Week, Month]);

// Az eventSettings változásainak figyelése és mentése a localStorage-ba
  watch(
  () => eventSettings.dataSource,
  (newData) => {
    localStorage.setItem("scheduleEvents", JSON.stringify(newData));
  },
  { deep: true }
);


// Felhasználói műveletek kiírása
function alertingSystem(content, severity,timeout){
  messageState.content = content;
  messageState.severity = severity;
  messageState.showMessage = true;
  setTimeout(() => {
  messageState.showMessage = false;
}, timeout);
}

// Eseménykezelő az események frissítésére
function onActionComplete(args) {
  try {
    if (args.requestType === "eventCreated") {
    alertingSystem("Munkaidő bejegyzés sikeresen hozzáadva!","Success",2500);
    // Új esemény(ek) hozzáadása a meglévő listához
    eventSettings.dataSource.push(...args.data);
  } else if (args.requestType === "eventChanged") {
    alertingSystem("Munkaidő bejegyzés sikeresen frissítve!","Info",2500);
    // Módosított események frissítése a dataSource-ban
    args.data.forEach((updatedEvent) => {
      const index = eventSettings.dataSource.findIndex(
        (event) => event.Id === updatedEvent.Id
        
      );
      if (index !== -1) {
        eventSettings.dataSource[index] = { ...updatedEvent };
      }
    });
  } else if (args.requestType === "eventRemoved") {
    alertingSystem("Munkaidő bejegyzés sikeresen törölve!","Warning",3000);
    // Törölt események eltávolítása a dataSource-ból
    args.data.forEach((deletedEvent) => {
      const index = eventSettings.dataSource.findIndex(
        (event) => event.Id === deletedEvent.Id
      );
      if (index !== -1) {
        eventSettings.dataSource.splice(index, 1);
      }
    });
  }
  } catch (error) {
    alertingSystem("Hiba lépett fel: " +error,"Error",10000);
  }
  
}
</script>

<style>
@import '../node_modules/@syncfusion/ej2-base/styles/material.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/material.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material.css';
@import '../node_modules/@syncfusion/ej2-dropdowns/styles/material.css';
@import '../node_modules/@syncfusion/ej2-inputs/styles/material.css';
@import '../node_modules/@syncfusion/ej2-navigations/styles/material.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material.css';
@import '../node_modules/@syncfusion/ej2-vue-schedule/styles/material.css';
@import "../node_modules/@syncfusion/ej2-vue-notifications/styles/message/material.css";
</style>
