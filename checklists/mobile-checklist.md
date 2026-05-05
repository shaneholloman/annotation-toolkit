# Mobile Checklist

This checklist summarizes accessibility considerations specific to iOS and Android, drawing from [Web Content Accessibility Guidelines (WCAG) 2.2](https://www.w3.org/TR/WCAG22/), the [W3C Guidance on Applying WCAG 2.2 to Mobile Applications](https://www.w3.org/TR/wcag2mobile-22/), and best practices identified by GitHub.

Native mobile has its own set of accessibility patterns, APIs, and user expectations that don't map cleanly to the web. Use this checklist when designing or building **native mobile experiences** on iOS and Android. You don't need to also run through the [Designer Checklist](./designer-checklist.md) or [Engineering Checklist](./engineering-checklist.md) for a native app.

The one exception: if part of your app uses a **web view**, apply the web checklists to that content. Flag those areas with a [View Context Stamp](../tutorials/mobile-annotations.md#view-context-stamps-and-details) so the handoff is explicit. Examples of web view content are an in-app browser, an OAuth flow, embedded docs, or any HTML-rendered content.

For per-interaction guidance, pair this checklist with the [User Interactions tutorial](../tutorials/user-interactions.md) and the [Mobile annotations tutorial](../tutorials/mobile-annotations.md). Use these when you're specifying individual interactions or framing a design review. The mobile annotations tutorial also includes a list of [design considerations](../tutorials/mobile-annotations.md#design-considerations) that surface the right questions before you start annotating.

For audit work, refer to the [Mobile-WCAG Mapping (Internal only)](https://github.com/github/accessibility-audit-guide/blob/main/mobile/mobile-wcag-map.md). Mobile changes how some WCAG Success Criteria apply, with a few being partial or having non-obvious scoping, and the mapping document captures those differences in one place so audits stay consistent.

**Further reading:**
- [Mobile annotations tutorial](../tutorials/mobile-annotations.md)
- [User Interactions tutorial](../tutorials/user-interactions.md)
- [Guidance on Applying WCAG 2.2 to Mobile Applications - W3C](https://www.w3.org/TR/wcag2mobile-22/)
- [Guidance on Applying WCAG 2 to Non-Web ICT (WCAG2ICT) - W3C](https://www.w3.org/TR/wcag2ict-22/)
- [Accessibility documentation - Appt](https://appt.org/en/docs)
- [Apple Human Interface Guidelines: Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility)
- [Material Design: Accessibility](https://m3.material.io/foundations/accessible-design/overview)

---

## 1. Color

- [ ] **Meaning is not conveyed with color alone**
	- Status, severity, and selection indicators should also use a label, icon, shape, or text treatment.
- [ ] **Color contrast is observed across both light and dark modes**
	- Functional text has a **4.5:1** contrast ratio (or higher) with its background.
	- Functional graphics (icons, status indicators, control identification) have a **3:1** contrast ratio (or higher).
	- Verify both system themes since gradients and elevation tints can shift contrast.
- [ ] **Designs respect user color settings**
	- Account for high contrast modes, [Smart Invert (iOS)](https://support.apple.com/guide/iphone/change-display-and-text-size-iph3e2e1fb0/ios), and [color correction (Android)](https://support.google.com/accessibility/android/answer/11183305?hl=en-GB&sjid=1525872318388975084-NC#zippy=%2Cuse-colour-correction:~:text=Colour%20correction%20and%20grayscale%20settings%20help%20your%20device%20compensate%20for%20colour%20blindness.%C2%A0). Don't bake essential information into images that get inverted.

### Suggested Tools

- [Check color contrast - Figma Docs](https://help.figma.com/hc/en-us/articles/360041003774-Apply-paints-with-the-color-picker#h_01JQF1T71AC72G6VDXN27B77V0)
- [Web Color Contrast Checker - WebAIM](https://webaim.org/resources/contrastchecker/)
- [Colour Contrast Analyzer for Mac and Windows - TPGi](https://www.tpgi.com/color-contrast-checker/)

---

## 2. Hierarchy

- [ ] **Each screen has at most one Title**
	- Not every screen needs a title, but if one is needed, only one is used. Titles live in the top app bar (Android) or navigation bar (iOS) and are announced on view transition. Note: in WCAG terms, [SC 2.4.2 Page Titled](https://www.w3.org/WAI/WCAG22/Understanding/page-titled.html) applies to the application as a whole on native, but the per-screen title is still the right pattern for orientation.
- [ ] **Headings are used sparingly and only when they help structure the screen**
	- Unlike the web, mobile screens often don't need headings. Use them when content has clear sections that benefit from rotor or [TalkBack](https://support.google.com/accessibility/android/answer/6283677) heading navigation.
- [ ] **Layouts favor a simple, predictable flow**
	- On some frameworks like React Native, the operating system determines reading order by scanning the screen roughly top-to-bottom, left-to-right, and there's no API to override it. Designs that follow a clear vertical flow narrate correctly regardless of framework, and they're easier for everyone to scan visually too.
- [ ] **Reading order matches visual order**
	- Screen readers narrate in view-tree order. Walk the screen with the [VoiceOver](https://support.apple.com/guide/iphone/turn-on-and-practice-voiceover-iph3e2e415f/ios) rotor (iOS) and TalkBack swipe gestures (Android) to confirm that headings, links, form controls, and landmarks surface in the right order. This is especially important on cards, carousels, and bottom sheets.
- [ ] **Grouping is annotated**
	- Use a Mobile Grouping Stamp to mark elements that should be announced as one unit (avatar + name + timestamp on a comment, for example). This includes any content block that should read as a single unit rather than several separate stops.

### Annotations that can help
- [Mobile Structure Stamp](../tutorials/mobile-annotations.md#mobile-structure-stamp)
- [Mobile Grouping Stamp](../tutorials/mobile-annotations.md#mobile-grouping-stamp)

### Exercises

Sketch out the focus order with arrows before annotating. If the line zig-zags, simplify the layout or regroup elements.

---

## 3. Content (Label, Value, Role, Hint)

- [ ] **Every interactive element has a Label**
	- Labels help people understand the purpose of a control. They're useful for everyone and especially important for people with cognitive disabilities.
	- For [Voice Control](https://support.apple.com/guide/iphone/use-voice-control-iph2c21a3c88/ios) and [Voice Access](https://support.google.com/accessibility/android/answer/6151848) users, the visible label text must be programmatically associated with the control so users can speak the name they see. Per [SC 2.5.3 Label in Name](https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html).
	- For elements without visible text (icon-only buttons, avatars, etc.), be explicit in your annotations about which label belongs to which control.
- [ ] **Stateful controls have a Value**
	- Toggles, sliders, segmented controls, and inputs need a current value (e.g., "On", "75%", "@octocat").
- [ ] **Roles and traits are specified per platform**
	- iOS uses traits (button, header, adjustable, selected). Android uses roles and state descriptions. Annotate both if you ship to both.
- [ ] **Hints are used only for non-obvious interactions**
	- Don't put critical info in hints. Users can disable them, and they're announced last.
- [ ] **Decorative elements are hidden from assistive tech**
	- Mark with `accessibilityElementsHidden` (iOS) or `importantForAccessibility="no"` (Android). If you're not sure whether something is decorative, walk through the [W3C alt-text decision tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) — most images aren't truly decorative.
- [ ] **Language of the app is set, and any sections in a different language are identified**
	- On native, [SC 3.1.1 Language of Page](https://www.w3.org/WAI/WCAG22/Understanding/language-of-page.html) applies to the application as a whole, not individual screens. For inline text in a different language, identify it programmatically per [SC 3.1.2 Language of Parts](https://www.w3.org/WAI/WCAG22/Understanding/language-of-parts.html).

### Annotations that can help
- [Mobile Details](../tutorials/mobile-annotations.md#mobile-details)
- [View Context Stamps and Details](../tutorials/mobile-annotations.md#view-context-stamps-and-details) for web views, user-generated content, and non-native content

---

## 4. Images, graphics, and other media

- [ ] **Functional images and icon-only buttons have an accessible label**
	- Decorative images should be hidden from screen readers, not given empty labels that still get focused.
- [ ] **Important text is not baked into images**
	- It can't be translated, scaled with Dynamic Type, or copied.
- [ ] **Video and audio include captions, transcripts, or audio descriptions where applicable**
- [ ] **Auto-playing or scrolling content can be paused, stopped, or hidden**
	- Includes auto-advancing carousels, looping animations, and live previews.
- [ ] **Nothing flashes more than 3 times per second**

### Resources

- [Alt Text Guide - GitHub Workplace Accessibility](https://github.com/github/workplace-accessibility/blob/main/resources/content-accessibility/alt-text-guide.md)
- [W3C alt-text decision tree](https://www.w3.org/WAI/tutorials/images/decision-tree/)

---

## 5. Interactivity and touch targets

- [ ] **Touch targets are at least 44x44 pt (iOS) / 48x48 dp (Android)**
	- This meets [WCAG 2.2 SC 2.5.8 Target Size (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html) of 24x24 CSS pixels with healthy margin, and aligns with both platforms' HIG guidance.
	- Targets that let users select a value spatially based on position within the target (sliders, color pickers, map pins inside a draggable region) are considered a single target for this SC.
- [ ] **Adjacent targets have enough spacing**
	- Aim for at least 8 pt/dp between tappable elements to prevent mis-taps.
- [ ] **Buttons and links are used appropriately**
	- Buttons trigger actions. Links navigate. Don't style one as the other. When annotating, consider the action performed when interacting with each element, and ensure that the right role is applied.
- [ ] **Each button and link has a unique, descriptive name**
	- Avoid repeated "More", "Open", or "View" labels in lists. Include context (e.g., "Open pull request 142").
- [ ] **Disabled buttons are avoided**
	- Prefer an inactive style with feedback explaining what's missing, or hide the action until it's available.

### Annotations that can help
[Mobile Button Stamp](../tutorials/mobile-annotations.md#mobile-button-stamp)

### Resources

- [Getting To The Bottom Of Minimum WCAG-Conformant Interactive Element Size - Smashing Magazine](https://www.smashingmagazine.com/2024/07/getting-bottom-minimum-wcag-conformant-interactive-element-size/)
- [Apple HIG: Buttons](https://developer.apple.com/design/human-interface-guidelines/buttons)
- [Material Design: Touch targets](https://m3.material.io/foundations/designing/structure#1da3138c-2b54-4f5b-9c88-dbf6e6b9c12a)

---

## 6. Gestures and motion

The [User Interactions tutorial](../tutorials/user-interactions.md#touch-gesture) groups touch gestures into three tiers: 

1. **Basic** (single tap, double tap, long press), 
2. **Specialized** (swipe, drag, tap-and-hold), and 
3. **Advanced** (multi-finger, pinch, rotate, path-based). 

Use this section to verify each tier is used appropriately.

- [ ] **Primary actions use Basic gestures**
	- Single tap is the most accessible. Reserve it for the most important interactions on a screen.
- [ ] **Specialized gestures are used sparingly and have a single-point alternative**
	- Pinch-to-zoom, swipe-to-delete, swipe-to-reveal-actions, and tap-and-hold all need a button or menu equivalent. Required by [SC 2.5.1 Pointer Gestures](https://www.w3.org/WAI/WCAG22/Understanding/pointer-gestures.html).
- [ ] **Advanced gestures are avoided unless essential**
	- Multi-finger taps, two-finger rotation, and path-based gestures should not be the only way to do something. If they are essential, document the [accessible alternatives](https://tetralogical.com/blog/2023/03/17/foundations-pointer-gestures/#examples-of-accessible-alternatives).
- [ ] **Drag-and-drop has a non-dragging alternative**
	- Per [SC 2.5.7 Dragging Movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html). Reorder lists with up/down buttons, move items with a menu, etc.
- [ ] **Custom gestures are exposed as VoiceOver and TalkBack Custom Actions**
	- So screen reader users can discover and trigger them without performing the gesture.
- [ ] **Animations and transitions respect [Reduce Motion](https://support.apple.com/guide/iphone/customize-onscreen-motion-iph0b691d3ed/ios)**
	- Honor `UIAccessibility.isReduceMotionEnabled` (iOS) and `Settings.Global.TRANSITION_ANIMATION_SCALE` (Android). Replace large motion with cross-fades or instant transitions.
- [ ] **Pointer cancellation is supported**
	- Per [SC 2.5.2 Pointer Cancellation](https://www.w3.org/WAI/WCAG22/Understanding/pointer-cancellation.html), trigger actions on the up-event so users can drag off a control to cancel.

### Annotations that can help
- [Touch gesture (User Interactions)](../tutorials/user-interactions.md#touch-gesture)
- [User Interactions tutorial](https://gh.io/annotation-tutorial-user-interactions)

---

## 7. Device settings and platform behavior

Mobile users rely heavily on system-level settings to make their device usable. The [Device setting annotations](../tutorials/user-interactions.md#device-setting) cover the most common ones. Designs need to respect these settings, not fight them.

- [ ] **Both portrait and landscape orientations are supported**
	- Per [SC 1.3.4 Orientation](https://www.w3.org/WAI/WCAG22/Understanding/orientation.html), don't lock orientation unless it's essential (e.g., a piano app). Many users mount their device in a fixed orientation for accessibility reasons. _All functionality and content must remain available across orientations._
- [ ] **Layouts adapt to [iOS' Dynamic Type](https://developer.apple.com/documentation/uikit/scaling-fonts-automatically) and [Android's Font Size Scaling](https://support.google.com/accessibility/android/answer/11183305)**
	- Test at the largest accessibility text sizes per [SC 1.4.4 Resize Text](https://www.w3.org/WAI/WCAG22/Understanding/resize-text.html). Content should reflow without truncation or overlap.
- [ ] **Viewport zoom and viewport resize don't break the layout**
	- Magnifier, browser zoom inside web views, and split-screen / multitasking on tablets all change the available viewport. No content overlap, no obscured controls.
- [ ] **Reduce Motion is honored**
	- Annotate any motion-heavy transition with the Reduced motion device setting so engineering knows to gate it.
- [ ] **Voice Control labels match visible text**
	- Voice Control on iOS and Voice Access on Android let users speak the name of a control. The visible text and the programmatic accessibility label must match — if they're visually similar but programmatically different (or vice versa), voice users can't activate the control.
- [ ] **Functionality does not require device motion to operate**
	- Shake-to-undo, tilt-to-scroll, and similar interactions need a button equivalent and should be possible to disable. Per [SC 2.5.4 Motion Actuation](https://www.w3.org/WAI/WCAG22/Understanding/motion-actuation.html).

### Annotations that can help
[Device setting (User Interactions)](../tutorials/user-interactions.md#device-setting): Reduced motion, Viewport zoom, Voice input, Viewport resize, Orientation, Shake device

---

## 8. Forms

- [ ] **Inputs have a persistent visible label, not just a placeholder**
	- Per [SC 3.3.2 Labels or Instructions](https://www.w3.org/WAI/WCAG22/Understanding/labels-or-instructions.html).
- [ ] **Keyboard type matches the input**
	- Email, number, phone, URL keyboards reduce errors and effort.
- [ ] **Autofill, autocomplete, and content type hints are set**
	- iOS `textContentType`, Android `autofillHints`. Helps password managers and reduces redundant entry per [SC 3.3.7 Redundant Entry](https://www.w3.org/WAI/WCAG22/Understanding/redundant-entry.html) and [SC 1.3.5 Identify Input Purpose](https://www.w3.org/WAI/WCAG22/Understanding/identify-input-purpose.html).
- [ ] **Errors are announced, indicated with text, and tied to the field**
	- Identify what went wrong using text, not just color or an icon.
	- When the fix is determinable, also suggest how to correct it (e.g., "Email must include @"). Per [SC 3.3.1 Error Identification](https://www.w3.org/WAI/WCAG22/Understanding/error-identification.html) and [SC 3.3.3 Error Suggestion](https://www.w3.org/WAI/WCAG22/Understanding/error-suggestion.html).
	- Use live regions or accessibility focus so screen reader users hear errors when they're announced.
- [ ] **Legal, financial, or test submissions can be reviewed, corrected, or reversed**
	- Per [SC 3.3.4 Error Prevention (Legal, Financial, Data)](https://www.w3.org/WAI/WCAG22/Understanding/error-prevention-legal-financial-data.html).

### Annotations that can help
[Form Element](https://gh.io/annotation-tutorial-form-element)

---

## 9. Layout

- [ ] **Layouts adapt across device sizes**
	- Cover small phones, large phones, foldables, and tablets. Verify safe areas, notches, and home indicators don't obscure content.
- [ ] **Content reflows without loss of information or functionality**
	- On native, [SC 1.4.10 Reflow](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html) applies through WCAG2ICT guidance: when users resize text via Dynamic Type or font scaling, content should reflow without requiring scrolling in two dimensions for primary content. Tables, image galleries, and similar are reasonable exceptions.
- [ ] **Persistent UI (nav, help, status) appears in consistent locations across screens**
	- Per [SC 3.2.3 Consistent Navigation](https://www.w3.org/WAI/WCAG22/Understanding/consistent-navigation.html) and [SC 3.2.6 Consistent Help](https://www.w3.org/WAI/WCAG22/Understanding/consistent-help.html).
- [ ] **Web view content (only) supports text spacing overrides**
	- [SC 1.4.12 Text Spacing](https://www.w3.org/WAI/WCAG22/Understanding/text-spacing.html) applies only inside a web view on native. For embedded HTML content, verify line height up to 1.5x font size, paragraph spacing up to 2x, letter spacing up to 0.12x, and word spacing up to 0.16x. Native UI is governed by Dynamic Type / font scaling instead (see §7).

---

## 10. Touch and keyboard navigation

- [ ] **Every touch interaction is reachable and operable with an external keyboard**
	- Bluetooth keyboards, [iOS Switch Control](https://support.apple.com/en-us/119835), and [Android Switch Access](https://support.google.com/accessibility/android/answer/6122836) all rely on a sensible focus order. Per [SC 2.1.1 Keyboard](https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html), the keyboard interface here means any keyboard or keyboard substitute, not just a physical device.
- [ ] **A visible focus indicator appears when navigating with a keyboard or switch**
	- Per [SC 2.4.7 Focus Visible](https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html) and [SC 2.4.11 Focus Not Obscured (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html).
- [ ] **Focus order is annotated and matches the visual flow**
	- Avoid focus that jumps back and forth across the screen.
- [ ] **There are no focus traps**
	- Modals, sheets, and overlays must let users dismiss and return focus to a logical place. Per [SC 2.1.2 No Keyboard Trap](https://www.w3.org/WAI/WCAG22/Understanding/no-keyboard-trap.html).
- [ ] **Custom controls expose proper accessibility actions**
	- A custom slider should respond to increment/decrement actions, not just drag.
- [ ] **Keyboard shortcuts don't override platform or assistive tech shortcuts**
	- Check against VoiceOver, TalkBack, Switch Control, and Voice Control shortcut tables before claiming a key combo. See the [Keyboard shortcut annotation guidance](../tutorials/user-interactions.md#keyboard-shortcut).
- [ ] **No single-character shortcuts are introduced for new features**
	- Per [SC 2.1.4 Character Key Shortcuts](https://www.w3.org/WAI/WCAG22/Understanding/character-key-shortcuts.html). If you must, allow users to remap or disable them. Note: long-press gestures and platform-provided accessibility features don't meet WCAG's definition of a keyboard shortcut, so they're not in scope here.

### Annotations that can help
- [Ordering](https://gh.io/annotation-tutorial-ordering) (Focus Order, Arrow Stop)
- [Keyboard shortcut (User Interactions)](../tutorials/user-interactions.md#keyboard-shortcut)

---

## 11. Platform parity

- [ ] **Stock platform controls are preferred over custom ones**
	- A native segmented control, switch, or date picker comes with accessibility behavior built in. Before designing a custom control, check whether a standard one would do the job.
- [ ] **iOS and Android specs are both annotated when shipping to both platforms**
	- Use the iOS (blue) and Android (green) variants of mobile annotations to make platform differences visible on the canvas.
- [ ] **Stock components use the platform's native accessibility behavior**
	- When using a system component, toggle "Follow stock native pattern" in your Mobile Details annotation and link to the platform docs.
- [ ] **Platform conventions are respected**
	- Back navigation, modal dismissal, and primary action placement differ between iOS and Android. Don't force one platform's pattern onto the other.
- [ ] **Components are identified consistently across the family of related apps**
	- If your team ships a suite of apps (e.g., GitHub Mobile + GitHub for iPad + an extension app), [SC 3.2.4 Consistent Identification](https://www.w3.org/WAI/WCAG22/Understanding/consistent-identification.html) and [SC 3.2.3 Consistent Navigation](https://www.w3.org/WAI/WCAG22/Understanding/consistent-navigation.html) apply across the set, not just within a single app. The same icon should mean the same thing everywhere.
- [ ] **Web views, user-generated content, and non-native content are flagged**
	- Use [View Context Stamps](../tutorials/mobile-annotations.md#view-context-stamps-and-details) so testers know where annotation handoff happens. For web view content specifically, run it through the [Designer](./designer-checklist.md) and [Engineering](./engineering-checklist.md) checklists.

---

## 12. Platform functions

The [Platform function annotations](../tutorials/user-interactions.md#platform-function) flag built-in behaviors that can disorient users if they're not designed carefully.

- [ ] **Loading states have clear visual and programmatic feedback**
	- Use a live region announcement, not just a spinner. See [Mobile Live Region Announcement](../tutorials/mobile-annotations.md#mobile-live-region-announcement). Per [SC 4.1.3 Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html).
- [ ] **Automatic transitions are avoided or controllable**
	- System-initiated screen, state, or content changes should be paused, slowed, or dismissible. Common offenders: onboarding carousels, auto-advancing tutorials. Per [SC 3.2.1 On Focus](https://www.w3.org/WAI/WCAG22/Understanding/on-focus.html) and [SC 3.2.2 On Input](https://www.w3.org/WAI/WCAG22/Understanding/on-input.html).
- [ ] **Opening external content is announced**
	- "Opens in Safari", "Opens in browser", or "Opens in a new window" warnings prevent disorientation, especially when leaving an app for a web view.
- [ ] **Time limits can be turned off, adjusted, or extended**
	- Exceptions: real-time events (auctions, live games) or time limits beyond 20 hours. Per [SC 2.2.1 Timing Adjustable](https://www.w3.org/WAI/WCAG22/Understanding/timing-adjustable.html).
- [ ] **Cognitive function tests have an accessible alternative**
	- CAPTCHA puzzles, math challenges, and memory checks need an alternate path. Per [SC 3.3.8 Accessible Authentication (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/accessible-authentication-minimum.html). Note: passwords used to unlock the underlying platform (device passcode, biometric unlock) are out of scope for this requirement at the app level.

### Annotations that can help
- [Platform function (User Interactions)](../tutorials/user-interactions.md#platform-function): Loading, Automatic transition, Open in new tab, Time limit, Cognitive puzzle
- [Mobile Live Region Announcement](../tutorials/mobile-annotations.md#mobile-live-region-announcement)

---

## 13. Notifications and live updates

- [ ] **Status changes are announced via Live Regions**
	- Search results, form validation, toast notifications, and async updates need an accessibility announcement so screen reader users don't miss them. Per [SC 4.1.3 Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html).
- [ ] **`polite` vs `assertive` is chosen intentionally**
	- Use `assertive` only for critical info (errors, blocking states). Default to `polite`.
- [ ] **Off-screen and dynamically appearing content has defined accessibility behavior**
	- When new content appears (a bottom sheet opens, a card is inserted, a banner slides in), decide whether focus should move to it, whether it should be announced, and whether off-screen elements are hidden from assistive tech until they're visible.
- [ ] **Auto-dismissing UI can be paused or extended**
	- Snackbars, toasts, and OTP inputs need an accessible alternative.

### Annotations that can help
[Mobile Live Region Announcement](../tutorials/mobile-annotations.md#mobile-live-region-announcement)

---

## Testing

- [ ] **Tested with VoiceOver on a real iOS device**
- [ ] **Tested with TalkBack on a real Android device**
- [ ] **Tested with the largest Dynamic Type / font scaling setting**
- [ ] **Tested with Reduce Motion enabled**
- [ ] **Tested in both light and dark mode, and in both portrait _and_ landscape**
- [ ] **Tested with Voice Control (iOS) and Voice Access (Android)**
- [ ] **Tested with an external keyboard or Switch Control / Switch Access**

### Suggested Tools

- [Accessibility Inspector - Apple](https://developer.apple.com/documentation/accessibility/accessibility-inspector)
- [Accessibility Scanner - Google](https://support.google.com/accessibility/android/answer/6376570)
- [Appium accessibility checks](https://appium.io/docs/en/2.0/)
- [Espresso Accessibility Checks - Android](https://developer.android.com/training/testing/espresso/accessibility-checking)

---

## Additional Resources

- [Mobile annotations tutorial](../tutorials/mobile-annotations.md)
- [User Interactions tutorial](../tutorials/user-interactions.md)
- [Accessibility documentation - Appt](https://appt.org/en/docs)
- [Guidance on Applying WCAG 2.2 to Mobile Applications - W3C](https://www.w3.org/TR/wcag2mobile-22/)
- [Guidance on Applying WCAG 2 to Non-Web ICT (WCAG2ICT) - W3C](https://www.w3.org/TR/wcag2ict-22/)
- [Mobile-WCAG Mapping - GitHub Accessibility Audit Guide (Internal only)](https://github.com/github/accessibility-audit-guide/blob/main/mobile/mobile-wcag-map.md)
- [Apple Accessibility Programming Guide](https://developer.apple.com/accessibility/)
- [Android Accessibility developer guide](https://developer.android.com/guide/topics/ui/accessibility)
- [Does WCAG 2.2 apply to native apps - TetraLogical](https://tetralogical.com/blog/2024/07/18/wcag2ict/)
- [Getting To The Bottom Of Minimum WCAG-Conformant Interactive Element Size - Smashing Magazine](https://www.smashingmagazine.com/2024/07/getting-bottom-minimum-wcag-conformant-interactive-element-size/)
- [Foundations: pointer gestures - TetraLogical](https://tetralogical.com/blog/2023/03/17/foundations-pointer-gestures/)
- [Foundations: Keyboard accessibility - TetraLogical](https://tetralogical.com/blog/2025/05/09/foundations-keyboard-accessibility/)
