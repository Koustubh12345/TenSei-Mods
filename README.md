# 🌌 TenSei Mods V1.0
<img width="2171" height="1220" alt="TenSeiMods_Banner" src="https://i.postimg.cc/NF5jPJSr/23419-modified.png" />

**TenSei Mods** is an advanced, root-only customization suite specifically engineered for **Oplus, ColorOS, OxygenOS, and RealmeUI** devices and ports. It combines high-performance Smali logic with a premium "Atmospheric" glass-morphism UI.

### Download
Download from the [releases page](https://github.com/Koustubh12345/TenSei-Mods/releases)

### Screenshots
<img width="1308" height="2813" alt="TenSei_Main" src="https://i.postimg.cc/SxHwbZ5z/IMG-20260321_184252_106.jpg" />
<img width="1308" height="2813" alt="Statusbar_Mods" src="https://i.postimg.cc/BbZzBR8r/IMG-20260321_184252_133.jpg" />
<img width="1308" height="2813" alt="Misc_Mods" src="https://i.postimg.cc/RVDYwsvj/IMG-20260321_184252_398.jpg" />
<img width="1308" height="2813" alt="Dark_Mode" src="https://i.postimg.cc/zB82q4kz/IMG-20260321_184252_220.jpg" />

### Instructions:
- Download and install the `TenSeiModsModule`
- Open the app and grant **Root Access** (Magisk or KernelSU).
- Select your desired modifications from the menu.
- - Dsv must be installed or patched`
- Click **"Apply Changes"** to restart SystemUI and finalize settings.

---

### Community & Updates
Join the official [Telegram Channel](https://t.me/Tenseimods) for queries and updates.

Modified by **[TenSei てんせい](https://t.me/Tenseimods)** with added features and UI changes.


### ⚠️ IMPORTANT: Screenshot Restriction Patch
The **"Remove Screenshot Restriction"** switch in the app will not work by default. To enable this feature, you must manually patch your `framework.jar`.

**Target File:** `/system/framework/framework.jar`  
**Target Class:** `Landroid/view/Window;`

#### Step-by-Step Patch:
1. Decompile `framework.jar`.
2. Open `android/view/Window.smali`.
3. Locate the method `.method public whitelist setFlags(II)V`.
4. Replace the **entire** method with the following code:

```smali
.method public whitelist setFlags(II)V
    .registers 6
    .param p1, "flags"  # I
    .param p2, "mask"  # I

    invoke-virtual {p0}, Landroid/view/Window;->getContext()Landroid/content/Context;

    move-result-object v0

    if-nez v0, :cond_7

    goto :goto_1a

    :cond_7
    invoke-virtual {v0}, Landroid/content/Context;->getContentResolver()Landroid/content/ContentResolver;

    move-result-object v0

    const-string/jumbo v1, "remove_screenshot_restriction"

    const/4 v2, 0x0

    invoke-static {v0, v1, v2}, Landroid/provider/Settings$System;->getInt(Landroid/content/ContentResolver;Ljava/lang/String;I)I

    move-result v0

    if-nez v0, :cond_16

    goto :goto_1a

    :cond_16
    and-int/lit16 p2, p2, -0x2001

    and-int/lit16 p1, p1, -0x2001

    .line 1320
    :goto_1a
    invoke-virtual {p0}, Landroid/view/Window;->getAttributes()Landroid/view/WindowManager$LayoutParams;

    move-result-object v0

    .line 1321
    .local v0, "attrs":Landroid/view/WindowManager$LayoutParams;
    iget v1, v0, Landroid/view/WindowManager$LayoutParams;->flags:I

    not-int v2, p2

    and-int/2addr v1, v2

    and-int v2, p1, p2

    or-int/2addr v1, v2

    iput v1, v0, Landroid/view/WindowManager$LayoutParams;->flags:I

    .line 1322
    iget v1, p0, Landroid/view/Window;->mForcedWindowFlags:I

    or-int/2addr v1, p2

    iput v1, p0, Landroid/view/Window;->mForcedWindowFlags:I

    .line 1323
    invoke-virtual {p0, v0}, Landroid/view/Window;->dispatchWindowAttributesChanged(Landroid/view/WindowManager$LayoutParams;)V

    .line 1324
    return-void
.end method
