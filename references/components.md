# Component Recipes — Premium Patterns

Ready-to-adapt patterns for common premium UI components. These are starting points — always customize to the project's design system.

---

## Table of Contents
1. [Bottom Sheet](#bottom-sheet)
2. [Toast / Notification](#toast)
3. [Empty State](#empty-state)
4. [Search Bar](#search-bar)
5. [Toggle / Switch](#toggle)
6. [Progress Indicators](#progress)
7. [Avatar](#avatar)
8. [Badge / Chip](#badge)

---

## 1. Bottom Sheet {#bottom-sheet}

The bottom sheet is a defining interaction of premium mobile apps. It must feel native.

**Recommended:** Use `@gorhom/bottom-sheet` — it's the best implementation.

```javascript
import BottomSheet, { BottomSheetBackdrop, BottomSheetView } from '@gorhom/bottom-sheet';

function PremiumBottomSheet({ children, snapPoints = ['50%', '90%'] }) {
  const bottomSheetRef = useRef(null);

  const renderBackdrop = useCallback((props) => (
    <BottomSheetBackdrop
      {...props}
      disappearsOnIndex={-1}
      appearsOnIndex={0}
      opacity={0.7}  // Darker backdrop = more premium focus
    />
  ), []);

  return (
    <BottomSheet
      ref={bottomSheetRef}
      snapPoints={snapPoints}
      enablePanDownToClose
      backdropComponent={renderBackdrop}
      handleIndicatorStyle={{ backgroundColor: colors.border, width: 36 }}
      backgroundStyle={{ backgroundColor: colors.surface, borderRadius: 24 }}
    >
      <BottomSheetView style={{ paddingHorizontal: 24, paddingBottom: 32 }}>
        {children}
      </BottomSheetView>
    </BottomSheet>
  );
}
```

**Design rules:**
- Handle indicator: 36px wide, 4px tall, rounded, subtle color
- Corner radius: 20-24px (feels native)
- Backdrop: darkened with blur on iOS
- Always `enablePanDownToClose`

---

## 2. Toast / Notification {#toast}

Toasts should enter from the top (or bottom on Android) with a spring, and auto-dismiss.

```javascript
function Toast({ message, type = 'default', visible }) {
  const translateY = useSharedValue(-100);
  const opacity = useSharedValue(0);
  const insets = useSafeAreaInsets();

  useEffect(() => {
    if (visible) {
      translateY.value = withSpring(insets.top + 16, { damping: 20, stiffness: 300 });
      opacity.value = withSpring(1);
    } else {
      translateY.value = withSpring(-100);
      opacity.value = withTiming(0, { duration: 200 });
    }
  }, [visible]);

  const style = useAnimatedStyle(() => ({
    transform: [{ translateY: translateY.value }],
    opacity: opacity.value,
  }));

  const bgColor = {
    default: colors.surface2,
    success: '#166534',
    error: '#7f1d1d',
    warning: '#78350f',
  }[type];

  return (
    <Animated.View style={[styles.toast, { backgroundColor: bgColor }, style]}>
      <Text style={styles.toastText}>{message}</Text>
    </Animated.View>
  );
}

const styles = StyleSheet.create({
  toast: {
    position: 'absolute',
    left: 16,
    right: 16,
    paddingHorizontal: 16,
    paddingVertical: 12,
    borderRadius: 12,
    flexDirection: 'row',
    alignItems: 'center',
    zIndex: 9999,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 8 },
    shadowOpacity: 0.4,
    shadowRadius: 16,
    elevation: 8,
  },
  toastText: { color: '#fff', fontSize: 14, fontWeight: '500', flex: 1 },
});
```

---

## 3. Empty State {#empty-state}

Empty states are often the most neglected part of an app. A premium empty state is an opportunity to delight.

```javascript
function EmptyState({ icon, title, subtitle, actionLabel, onAction }) {
  return (
    <View style={styles.container}>
      {/* Icon container — use a slightly tinted circle */}
      <View style={styles.iconCircle}>
        {icon}
      </View>
      <Text style={styles.title}>{title}</Text>
      <Text style={styles.subtitle}>{subtitle}</Text>
      {actionLabel && (
        <Pressable style={styles.button} onPress={onAction}>
          <Text style={styles.buttonText}>{actionLabel}</Text>
        </Pressable>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 48,
  },
  iconCircle: {
    width: 72,
    height: 72,
    borderRadius: 36,
    backgroundColor: colors.surface2,
    alignItems: 'center',
    justifyContent: 'center',
    marginBottom: 20,
  },
  title: {
    fontSize: 20,
    fontWeight: '600',
    color: colors.text,
    textAlign: 'center',
    marginBottom: 8,
  },
  subtitle: {
    fontSize: 15,
    color: colors.text2,
    textAlign: 'center',
    lineHeight: 22,
    marginBottom: 24,
  },
  button: {
    backgroundColor: colors.accent,
    paddingHorizontal: 20,
    paddingVertical: 12,
    borderRadius: 10,
  },
  buttonText: {
    color: '#fff',
    fontWeight: '600',
    fontSize: 15,
  },
});
```

**Empty state copy rules:**
- Title: what's missing, not what the user did wrong ("No tracks yet" not "Nothing found")
- Subtitle: what they can do about it, with encouragement
- Action: direct CTA, not "OK"

---

## 4. Search Bar {#search-bar}

```javascript
function SearchBar({ value, onChangeText, placeholder = 'Search...' }) {
  const [focused, setFocused] = useState(false);
  const borderColor = useSharedValue(colors.border);

  useEffect(() => {
    borderColor.value = withTiming(focused ? colors.accent : colors.border, { duration: 150 });
  }, [focused]);

  const animatedBorder = useAnimatedStyle(() => ({
    borderColor: borderColor.value,
  }));

  return (
    <Animated.View style={[styles.container, animatedBorder]}>
      <SearchIcon size={16} color={focused ? colors.accent : colors.text3} />
      <TextInput
        style={styles.input}
        value={value}
        onChangeText={onChangeText}
        placeholder={placeholder}
        placeholderTextColor={colors.text3}
        onFocus={() => setFocused(true)}
        onBlur={() => setFocused(false)}
        returnKeyType="search"
        clearButtonMode="while-editing" // iOS clear button
      />
      {value.length > 0 && (
        <Pressable onPress={() => onChangeText('')} hitSlop={8}>
          <XCircle size={16} color={colors.text3} />
        </Pressable>
      )}
    </Animated.View>
  );
}

const styles = StyleSheet.create({
  container: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: colors.surface,
    borderRadius: 10,
    borderWidth: 1,
    paddingHorizontal: 12,
    paddingVertical: 10,
    gap: 8,
  },
  input: {
    flex: 1,
    fontSize: 15,
    color: colors.text,
    padding: 0, // Reset Android default padding
  },
});
```

---

## 5. Toggle / Switch {#toggle}

The native RN Switch is fine for settings. But for feature toggles inline, a custom one feels much more premium:

```javascript
function PremiumToggle({ value, onValueChange, size = 'md' }) {
  const translateX = useSharedValue(value ? 1 : 0);
  
  const SIZES = {
    sm: { track: { width: 36, height: 20 }, thumb: 16, padding: 2 },
    md: { track: { width: 48, height: 28 }, thumb: 22, padding: 3 },
  };
  const s = SIZES[size];
  
  useEffect(() => {
    translateX.value = withSpring(value ? s.track.width - s.thumb - s.padding * 2 : 0, {
      damping: 18,
      stiffness: 350,
    });
  }, [value]);

  const thumbStyle = useAnimatedStyle(() => ({
    transform: [{ translateX: translateX.value }],
  }));

  const trackColor = useAnimatedStyle(() => ({
    backgroundColor: withTiming(value ? colors.accent : colors.surface2, { duration: 200 }),
  }));

  return (
    <Pressable onPress={() => onValueChange(!value)} hitSlop={8}>
      <Animated.View style={[{
        width: s.track.width,
        height: s.track.height,
        borderRadius: s.track.height / 2,
        padding: s.padding,
        justifyContent: 'center',
      }, trackColor]}>
        <Animated.View style={[{
          width: s.thumb,
          height: s.thumb,
          borderRadius: s.thumb / 2,
          backgroundColor: '#fff',
          shadowColor: '#000',
          shadowOffset: { width: 0, height: 1 },
          shadowOpacity: 0.3,
          shadowRadius: 2,
          elevation: 2,
        }, thumbStyle]} />
      </Animated.View>
    </Pressable>
  );
}
```

---

## 6. Progress Indicators {#progress}

**Skeleton (preferred for content loading):**
See `mobile.md` for the shimmer skeleton implementation.

**Step Progress (onboarding, multi-step forms):**
```javascript
function StepProgress({ current, total }) {
  return (
    <View style={{ flexDirection: 'row', gap: 4 }}>
      {Array.from({ length: total }).map((_, i) => {
        const isActive = i <= current;
        return (
          <View
            key={i}
            style={{
              height: 3,
              flex: 1,
              borderRadius: 2,
              backgroundColor: isActive ? colors.accent : colors.border,
              // Smooth color transition with Reanimated in production
            }}
          />
        );
      })}
    </View>
  );
}
```

---

## 7. Avatar {#avatar}

```javascript
function Avatar({ uri, name, size = 40 }) {
  const initials = name
    .split(' ')
    .map(n => n[0])
    .slice(0, 2)
    .join('')
    .toUpperCase();

  return (
    <View style={{
      width: size,
      height: size,
      borderRadius: size / 2,
      backgroundColor: colors.surface2,
      overflow: 'hidden',
      alignItems: 'center',
      justifyContent: 'center',
    }}>
      {uri ? (
        <Image source={{ uri }} style={{ width: size, height: size }} />
      ) : (
        <Text style={{
          fontSize: size * 0.36,
          fontWeight: '600',
          color: colors.text2,
        }}>
          {initials}
        </Text>
      )}
    </View>
  );
}
```

---

## 8. Badge / Chip {#badge}

```javascript
// Status badge
function Badge({ label, variant = 'default' }) {
  const variants = {
    default: { bg: colors.surface2, text: colors.text2 },
    success: { bg: 'rgba(34, 197, 94, 0.15)', text: '#22c55e' },
    warning: { bg: 'rgba(245, 158, 11, 0.15)', text: '#f59e0b' },
    error:   { bg: 'rgba(239, 68, 68, 0.15)', text: '#ef4444' },
    accent:  { bg: 'rgba(124, 58, 237, 0.15)', text: colors.accent },
  };
  const style = variants[variant];
  
  return (
    <View style={{
      paddingHorizontal: 8,
      paddingVertical: 3,
      borderRadius: 6,
      backgroundColor: style.bg,
    }}>
      <Text style={{
        fontSize: 11,
        fontWeight: '600',
        color: style.text,
        letterSpacing: 0.3,
      }}>
        {label}
      </Text>
    </View>
  );
}

// Selectable chip (filter, tag)
function Chip({ label, selected, onPress }) {
  return (
    <Pressable
      onPress={onPress}
      style={({ pressed }) => ({
        paddingHorizontal: 14,
        paddingVertical: 8,
        borderRadius: 999,
        backgroundColor: selected ? colors.accent : colors.surface,
        borderWidth: 1,
        borderColor: selected ? colors.accent : colors.border,
        opacity: pressed ? 0.7 : 1,
      })}
    >
      <Text style={{
        fontSize: 13,
        fontWeight: selected ? '600' : '400',
        color: selected ? '#fff' : colors.text2,
      }}>
        {label}
      </Text>
    </Pressable>
  );
}
```
