# Android Call Blocker

##Step 1:
  - create a new project in Android Studio

##Step 2:
  - create an AIDL folder 
  - create a new package in `AIDL` folder name must be `com.android.internal.telephony`
  - create an `AIDL` file in that package name must be the `ITelephony.aidl`
  - add that code in `ITelephony.aidl`
```
  package com.android.internal.telephony;
  interface ITelephony {
    boolean endCall();
    void answerRingingCall();
    void silenceRinger();
  }
```
##Step 3:
  - now its time to block all incomming calls
  - Create a a `BroadcastReceiver` `class`
  - add that code to the class
```
package jovial.callblocker;

import android.app.Application;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.telephony.TelephonyManager;
import android.widget.Toast;

import com.android.internal.telephony.ITelephony;
import java.lang.reflect.Method;



public class CallReceiver extends BroadcastReceiver {
    public CallReceiver() {
    }

    @Override
    public void onReceive(Context context, Intent intent) {
        ITelephony telephonyService;
        TelephonyManager telephony = (TelephonyManager) context.getSystemService(Context.TELEPHONY_SERVICE);

        try {
            String state = intent.getStringExtra(TelephonyManager.EXTRA_STATE);
            String incomingNumber = intent.getStringExtra(TelephonyManager.EXTRA_INCOMING_NUMBER);

            Class c = Class.forName(telephony.getClass().getName());
            Method m = c.getDeclaredMethod("getITelephony");
            m.setAccessible(true);
            telephonyService = (ITelephony)m.invoke(telephony);
            telephonyService.endCall();

            /*
            if(state.equals(TelephonyManager.EXTRA_STATE_RINGING)){
                Toast.makeText(context,"Ringing State Number is -"+incomingNumber,Toast.LENGTH_SHORT).show();

            }
            if ((state.equals(TelephonyManager.EXTRA_STATE_OFFHOOK))){
                Toast.makeText(context,"Received State",Toast.LENGTH_SHORT).show();
            }
            if (state.equals(TelephonyManager.EXTRA_STATE_IDLE)){
                Toast.makeText(context,"Idle State",Toast.LENGTH_SHORT).show();
            }
            */

        }
        catch (Exception e){
            e.printStackTrace();
        }

    }
}

```
#try this out, facing any issue? let me know
