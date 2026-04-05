# Mobile UI Reference — React Native / Expo

Deep guidance for building premium mobile interfaces. Read this file when building any React Native or Expo screen.

---

## Table of Contents
1. [Project Setup Standards](#setup)
2. [Navigation Architecture](#navigation)
3. [Core Layout Patterns](#layout)
4. [Gesture Interactions](#gestures)
5. [Animation with Reanimated](#animation)
6. [Platform Differences (iOS vs Android)](#platform)
7. [Typography on Mobile](#typography)
8. [Color & Theming](#color)
9. [Performance Patterns](#performance)

---

## 1. Project Setup Standards {#setup}

Essential packages for a premium mobile app:
```bash
npx expo install \
  react-native-reanimated \
  react-native-gesture-handler \
  react-native-safe-area-context \
  react-native-screens \
  @react-navigation/native \
  @react-navigation/native-stack \
  @react-navigation/bottom-tabs \
  expo-haptics \
  expo-blur
```

**babel.config.js — Reanimated must be last plugin:**
```javascript
module.exports = {
  presets: ['babel-preset-expo'],
  plugins: ['react-native-reanimated/plugin'], // MUST be last
};
```

---

## 2. Navigation Architecture {#navigation}

### Stack Navigator (standard screen flow)
```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

// Premium feel: native header vs custom
<Stack.Navigator
  screenOptions={{
    headerStyle: { backgroundColor: colors.bg },
    headerTintColor: colors.text,
    headerTitleStyle: { fontFamily: 'YourFont-Semibold', fontSize: 17 },
    headerShadowVisible: false, // cleaner look
    contentStyle: { backgroundColor: colors.bg },
    animation: 'ios', // native slide feels best
  }}
>
```

### Bottom Tab Bar (app shell)
```javascript
// Premium tab bar: use a custom tabBar component for full control
<Tab.Navigator
  tabBar={(props) => <PremiumTabBar {...props} />}
>
```

**PremiumTabBar pattern:**
- Blur background (iOS: `expo-blur`, Android: solid surface color)
- Active indicator: animated pill or dot, not just color change
- Icon + label: 24pt icons, 10pt labels, bold when active
- Always respect bottom safe area inset
- Spring animation on tab switch (scale 1→1.1→1)

```javascript
function PremiumTabBar({ state, descriptors, navigation }) {
  const insets = useSafeAreaInsets();
  return (
    <View style={{ paddingBottom: insets.bottom, backgroundColor: colors.surface }}>
      {/* Tab items with Reanimated spring animations */}
    </View>
  );
}
```

---

## 3. Core Layout Patterns {#layout}

### Screen Template
```javascript
import { SafeAreaView } from 'react-native-safe-area-context';

function PremiumScreen() {
  return (
    <SafeAreaView style={{ flex: 1, backgroundColor: colors.bg }} edges={['top']}>
      {/* Header */}
      <View style={styles.header}>
        <Text style={styles.title}>Screen Title</Text>
      </View>
      {/* Content */}
      <ScrollView
        contentContainerStyle={{ paddingBottom: 32 }}
        scrollEventThrottle={16}
        showsVerticalScrollIndicator={false}
      >
        {/* Content here */}
      </ScrollView>
    </SafeAreaView>
  );
}
```

### Card Component
```javascript
function Card({ children, onPress, style }) {
  const scale = useSharedValue(1);
  
  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ scale: scale.value }],
  }));
  
  const handlePressIn = () => {
    scale.value = withSpring(0.97, { damping: 15, stiffness: 300 });
  };
  const handlePressOut = () => {
    scale.value = withSpring(1, { damping: 15, stiffness: 300 });
  };

  return (
    <Pressable onPressIn={handlePressIn} onPressOut={handlePressOut} onPress={onPress}>
      <Animated.View style={[styles.card, animatedStyle, style]}>
        {children}
      </Animated.View>
    </Pressable>
  );
}

const styles = StyleSheet.create({
  card: {
    backgroundColor: colors.surface,
    borderRadius: 12,
    padding: 16,
    borderWidth: StyleSheet.hairlineWidth,
    borderColor: colors.border,
  }
});
```

### List Item Pattern
```javascript
// Premium list items: clear hierarchy, generous touch target, subtle separator
function ListItem({ title, subtitle, icon, onPress, trailingText }) {
  return (
    <Pressable
      onPress={onPress}
      style={({ pressed }) => [
        styles.item,
        pressed && styles.itemPressed,
      ]}
    >
      {icon && <View style={styles.iconContainer}>{icon}</View>}
      <View style={styles.content}>
        <Text style={styles.title} numberOfLines={1}>{title}</Text>
        {subtitle && <Text style={styles.subtitle} numberOfLines={1}>{subtitle}</Text>}
      </View>
      {trailingText && <Text style={styles.trailing}>{trailingText}</Text>}
      <ChevronRight size={16} color={colors.text3} />
    </Pressable>
  );
}
```

---

## 4. Gesture Interactions {#gestures}

### Swipe to Delete / Action
```javascript
import { Gesture, GestureDetector } from 'react-native-gesture-handler';
import Animated, { useSharedValue, useAnimatedStyle, withSpring } from 'react-native-reanimated';

function SwipeableRow({ children, onDelete }) {
  const translateX = useSharedValue(0);
  const SWIPE_THRESHOLD = -80;

  const pan = Gesture.Pan()
    .activeOffsetX([-10, 10])
    .onUpdate((e) => {
      translateX.value = Math.max(e.translationX, SWIPE_THRESHOLD);
    })
    .onEnd(() => {
      if (translateX.value < SWIPE_THRESHOLD / 2) {
        translateX.value = withSpring(SWIPE_THRESHOLD);
      } else {
        translateX.value = withSpring(0);
      }
    });

  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ translateX: translateX.value }],
  }));

  return (
    <View>
      {/* Delete action behind */}
      <View style={styles.deleteAction}>
        <Text style={styles.deleteText}>Delete</Text>
      </View>
      <GestureDetector gesture={pan}>
        <Animated.View style={animatedStyle}>{children}</Animated.View>
      </GestureDetector>
    </View>
  );
}
```

### Pull to Dismiss (Modal)
Use `react-navigation`'s `presentation: 'modal'` with `gestureEnabled: true` — it handles this natively and it feels incredible on iOS.

---

## 5. Animation with Reanimated {#animation}

### Spring Presets
```javascript
// Bouncy — for playful apps, icon taps
const SPRING_BOUNCY = { damping: 10, stiffness: 300, mass: 0.8 };
// Smooth — for most UI transitions
const SPRING_SMOOTH = { damping: 20, stiffness: 300 };
// Snappy — for quick confirmations
const SPRING_SNAP = { damping: 25, stiffness: 500 };
// Gentle — for subtle state changes
const SPRING_GENTLE = { damping: 30, stiffness: 150 };
```

### Entrance Animations (List Items)
```javascript
// Stagger list items on mount — feels incredibly polished
function AnimatedListItem({ index, children }) {
  const opacity = useSharedValue(0);
  const translateY = useSharedValue(16);

  useEffect(() => {
    const delay = index * 50; // 50ms stagger between items
    opacity.value = withDelay(delay, withSpring(1, SPRING_SMOOTH));
    translateY.value = withDelay(delay, withSpring(0, SPRING_SMOOTH));
  }, []);

  const style = useAnimatedStyle(() => ({
    opacity: opacity.value,
    transform: [{ translateY: translateY.value }],
  }));

  return <Animated.View style={style}>{children}</Animated.View>;
}
```

### Loading Skeleton
```javascript
// Shimmer effect on skeleton — much better than a spinner
function Skeleton({ width, height, borderRadius = 8 }) {
  const shimmer = useSharedValue(0);

  useEffect(() => {
    shimmer.value = withRepeat(
      withTiming(1, { duration: 1200, easing: Easing.inOut(Easing.ease) }),
      -1,
      true
    );
  }, []);

  const animatedStyle = useAnimatedStyle(() => ({
    opacity: interpolate(shimmer.value, [0, 0.5, 1], [0.3, 0.6, 0.3]),
  }));

  return (
    <Animated.View
      style={[{ width, height, borderRadius, backgroundColor: colors.surface2 }, animatedStyle]}
    />
  );
}
```

---

## 6. Platform Differences {#platform}

### iOS-Specific Premium Patterns
```javascript
import { Platform, BlurView } from 'expo-blur';

// Blur headers and tab bars on iOS — it's a signature premium feel
{Platform.OS === 'ios' && (
  <BlurView intensity={80} tint="dark" style={StyleSheet.absoluteFillObject} />
)}

// Large titles on iOS — use headerLargeTitle in native stack
screenOptions={{ headerLargeTitle: true, headerTransparent: true }}

// Haptic feedback — use it deliberately
import * as Haptics from 'expo-haptics';
// On important actions: Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium)
// On success: Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)
// On tab switch: Haptics.selectionAsync()
// Do NOT overuse — only on intentional, significant interactions
```

### Android-Specific Considerations
```javascript
// Android ripple — customize it, don't leave default
android_ripple={{ color: 'rgba(255,255,255,0.08)', borderless: false }}

// Status bar — always configure it
import { StatusBar } from 'expo-status-bar';
<StatusBar style="light" backgroundColor="transparent" translucent />

// Edge-to-edge on Android 15+ — handle with care
```

---

## 7. Typography on Mobile {#typography}

```javascript
// Load custom fonts with expo-font
import { useFonts } from 'expo-font';

const [fontsLoaded] = useFonts({
  'Font-Regular': require('./assets/fonts/Font-Regular.ttf'),
  'Font-Medium': require('./assets/fonts/Font-Medium.ttf'),
  'Font-Semibold': require('./assets/fonts/Font-Semibold.ttf'),
  'Font-Bold': require('./assets/fonts/Font-Bold.ttf'),
});

// Type scale for mobile (slightly different from web due to screen distance)
export const typography = {
  xs:   { fontSize: 11, lineHeight: 16, letterSpacing: 0.3 },
  sm:   { fontSize: 13, lineHeight: 18, letterSpacing: 0.1 },
  base: { fontSize: 15, lineHeight: 22, letterSpacing: 0 },
  md:   { fontSize: 17, lineHeight: 24, letterSpacing: -0.2 },
  lg:   { fontSize: 20, lineHeight: 26, letterSpacing: -0.4 },
  xl:   { fontSize: 24, lineHeight: 30, letterSpacing: -0.6 },
  '2xl':{ fontSize: 30, lineHeight: 36, letterSpacing: -0.8 },
  '3xl':{ fontSize: 38, lineHeight: 44, letterSpacing: -1.0 },
};
```

---

## 8. Color & Theming {#color}

### Dark / Light Mode
```javascript
import { useColorScheme } from 'react-native';

const darkColors = {
  bg: '#0a0a0a',
  surface: '#111111',
  surface2: '#1a1a1a',
  border: '#262626',
  text: '#f5f5f5',
  text2: '#a3a3a3',
  text3: '#525252',
  accent: '#7c3aed',
};

const lightColors = {
  bg: '#fafaf9',
  surface: '#ffffff',
  surface2: '#f5f5f4',
  border: '#e7e5e4',
  text: '#0c0a09',
  text2: '#78716c',
  text3: '#a8a29e',
  accent: '#7c3aed',
};

export function useColors() {
  const scheme = useColorScheme();
  return scheme === 'dark' ? darkColors : lightColors;
}
```

---

## 9. Performance Patterns {#performance}

- **FlatList**: Always use for lists > 20 items. Set `keyExtractor`, `getItemLayout` when possible.
- **Images**: Always use `expo-image` instead of RN Image — it has smart caching.
- **Heavy components**: Wrap in `React.memo()` and use `useCallback` for handlers.
- **Avoid inline styles in render**: Define `StyleSheet.create()` outside component.
- **Avoid anonymous functions as props** in lists — they break memoization.
- **InteractionManager**: Defer heavy work until after animations complete.
```javascript
InteractionManager.runAfterInteractions(() => {
  // do heavy work here
});
```
