package com.learnium.RNDeviceInfo;

import android.app.Activity;
import android.os.Environment;
import android.util.Log;
import android.view.View;
import android.view.WindowInsets;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Properties;

/**
 * 全面屏工具类
 * @author  sin
 */
public class FullScreenUtils {

    /**
     * 判断是否是刘海屏
     * @return
     */
    public static boolean hasNotchScreen(Activity activity){
        if (hasNotchAtXiaoMi(activity) || hasNotchAtHuawei(activity) || hasNotchAtOPPO(activity)
                || hasNotchAtVivo(activity)){ //TODO 各种品牌
            return true;
        }

        return false;
    }

//    /**
//     * Android P 刘海屏判断
//     * @param activity
//     * @return
//     */
//    public static DisplayCutout isAndroidP(Activity activity){
//        View decorView = activity.getWindow().getDecorView();
//        if (decorView != null && android.os.Build.VERSION.SDK_INT >= 28){
//            WindowInsets windowInsets = decorView.getRootWindowInsets();
//            if (windowInsets != null)
//                return windowInsets.getDisplayCutout();
//        }
//        return null;
//    }

    /**
     * 小米刘海屏判断.
     * @return
     * @throws IllegalArgumentException if the key exceeds 32 characters
     */
    public static Boolean hasNotchAtXiaoMi(Activity activity) {
        String key = "ro.miui.notch";
        int result = 0;
            try {
                ClassLoader classLoader = activity.getClassLoader();
                @SuppressWarnings("rawtypes")
                Class SystemProperties = classLoader.loadClass("android.os.SystemProperties");
                //参数类型
                @SuppressWarnings("rawtypes")
                Class[] paramTypes = new Class[2];
                paramTypes[0] = String.class;
                paramTypes[1] = int.class;
                Method getInt = SystemProperties.getMethod("getInt", paramTypes);
                //参数
                Object[] params = new Object[2];
                params[0] = new String(key);
                params[1] = new Integer(0);
                result = (Integer) getInt.invoke(SystemProperties, params);

            } catch (ClassNotFoundException e) {
                e.printStackTrace();
                Log.e( "FullScreenUtils","hasNotchAtXiaoMi ClassNotFoundException");
            } catch (NoSuchMethodException e) {
                Log.e( "FullScreenUtils","hasNotchAtXiaoMi NoSuchMethodException");
            } catch (IllegalAccessException e) {
                Log.e( "FullScreenUtils","hasNotchAtXiaoMi IllegalAccessException");
            } catch (IllegalArgumentException e) {
                Log.e( "FullScreenUtils","hasNotchAtXiaoMi IllegalArgumentException");
            } catch (InvocationTargetException e) {
                Log.e( "FullScreenUtils","hasNotchAtXiaoMi InvocationTargetException");
            }
        return result == 1;
    }

    /**
     * 华为刘海屏判断
     * @return
     */
    public static boolean hasNotchAtHuawei(Activity activity) {
        boolean ret = false;
        try {
            ClassLoader classLoader = activity.getClassLoader();
            Class HwNotchSizeUtil = classLoader.loadClass("com.huawei.android.util.HwNotchSizeUtil");
            Method get = HwNotchSizeUtil.getMethod("hasNotchInScreen");
            ret = (boolean) get.invoke(HwNotchSizeUtil);
        } catch (ClassNotFoundException e) {
            Log.e("FullScreenUtils","hasNotchAtHuawei ClassNotFoundException");
        } catch (NoSuchMethodException e) {
            Log.e("FullScreenUtils","hasNotchAtHuawei NoSuchMethodException");
        } catch (Exception e) {
            Log.e("FullScreenUtils", "hasNotchAtHuawei Exception");
        } finally {
            return ret;
        }
    }

    public static final int VIVO_NOTCH = 0x00000020;//是否有刘海
    public static final int VIVO_FILLET = 0x00000008;//是否有圆角

    /**
     * VIVO刘海屏判断
     * @return
     */
    public static boolean hasNotchAtVivo(Activity activity) {
        boolean ret = false;
        try {
            ClassLoader classLoader = activity.getClassLoader();
            Class FtFeature = classLoader.loadClass("android.util.FtFeature");
            Method method = FtFeature.getMethod("isFeatureSupport", int.class);
            ret = (boolean) method.invoke(FtFeature, VIVO_NOTCH);
        } catch (ClassNotFoundException e) {
            Log.e( "FullScreenUtils","hasNotchAtVivo ClassNotFoundException");
        } catch (NoSuchMethodException e) {
            Log.e(  "FullScreenUtils","hasNotchAtVivo NoSuchMethodException");
        } catch (Exception e) {
            Log.e("FullScreenUtils","hasNotchAtVivo Exception");
        } finally {
            return ret;
        }
    }
    /**
     * OPPO刘海屏判断
     * @return
     */
    public static boolean hasNotchAtOPPO(Activity activity) {
        return  activity.getApplicationContext().getPackageManager().hasSystemFeature("com.oppo.feature.screen.heteromorphism");
    }

    /**
     * 判断是否为小米机型
     * @return
     */
    private static Boolean isXiaomi(){
        Properties prop= new Properties();
        try {
            prop.load(new FileInputStream(new File(Environment.getRootDirectory(), "build.prop")));
        } catch (IOException e) {
            Log.e("FullScreenUtils","juge isXiaomi IOException, file not found!");
        }
        return prop.getProperty("ro.miui.ui.version.code", null) != null
                || prop.getProperty("ro.miui.ui.version.name", null) != null
                || prop.getProperty("ro.miui.internal.storage", null) != null;
    }



}
