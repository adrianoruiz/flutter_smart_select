# smart_select

SmartSelect allows you to easily convert your usual form select or dropdown into dynamic page, popup dialog, or sliding bottom sheet with various choices input such as radio, checkbox, switch, chips, or even custom input. Supports single and multiple choice. Inspired by Smart Select component from [Framework7](https://framework7.io/).

## What's New in Version 4.0.0

- Customizable every part on modal widget (header, footer, searchbar, confirm button, searchbar toggle) using style configuration or widget builder
- Validate before confirm
- Auto search on type
- Accent marks handler on search
- Highlight search result
- Chips tile widget
- Grid choice layout
- Horizotal or vertical choice list scroll direction
- Simplify class name and enum
- Configurations supports `copyWith` and `merge`
- Use `StatefulWidget` as state management
- Easy shortcut to define configuration
- Soft depends to other package

## To Do

- Right-To-Left support
- Internally handle async choice items loader
- Custom search handler
- Choice items pagination (pull to refresh and pull to load more)

## Migration from 3.0.x to 4.x.x

- Simplify class name and enum

  - `SmartSelectTile` to `S2Tile`
  - `SmartSelectOption` to `S2Choice`
  - `SmartSelectChoiceConfig` to `S2ChoiceConfig`
  - `SmartSelectChoiceStyle` to `S2ChoiceStyle`
  - `SmartSelectChoiceType` to `S2ChoiceType`
  - `SmartSelectModalConfig` to `S2ModalConfig`
  - `SmartSelectModalStyle` to `S2ModalStyle`
  - `SmartSelectModalHeaderStyle` to `S2ModalHeaderStyle`
  - `SmartSelectModalType` to `S2ModalType`

- The parameter `builder` now is a collection of builder (`S2SingleBuilder` or `S2MultiBuilder`), instead use `tileBuilder` to create trigger tile widget.

- The parameters `dense`, `enabled`, `isLoading`, `isTwoLine`, `leading`, `loadingText`, `padding`, `selected`, `trailing` is removed from `SmartSelect` class, instead use `tileBuilder` and return `S2Tile` widget, it has all these parameters.

- The parameter `onChange` nows return `S2SingleState state` or `S2MultiState state` instead of `T value` or `List<T> value`

- The parameter `choiceConfig.useWrap` is removed, instead use `choiceConfig.layout = S2ChoiceLayout.wrap`

- The parameter `choiceConfig.builder` moved to `builder.choiceBuilder`

- The parameter `choiceConfig.titleBuilder` moved to `builder.choiceTitleBuilder`

- The parameter `choiceConfig.subtitleBuilder` moved to `builder.choiceSubtitleBuilder`

- The parameter `choiceConfig.secondaryBuilder` moved to `builder.choiceSecondaryBuilder`

- The parameter `choiceConfig.dividerBuilder` moved to `builder.choiceDividerBuilder`

- The parameter `choiceConfig.emptyBuilder` moved to `builder.choiceEmptybuilder`

- The parameter `choiceConfig.glowingOverscrollIndicatorColor` is removed, instead use `choiceConfig.overscrollColor`

- The parameter `choiceConfig.spacing` and `choiceConfig.runSpacing` moved to `choiceConfig.style.spacing` and `choiceConfig.style.runSpacing`

- The parameter `choiceConfig.useCheckmark` moved to `choiceConfig.style.showCheckmark`

- The parameter `choiceConfig.padding` moved to `choiceConfig.style.wrapperPadding`

- The default of grouped choice is not using sticky header now, instead use `groupBuilder` like this:

  ```dart
  dependencies:
    sticky_headers: "^0.1.8"
  ```

  ```dart
  import 'package:sticky_headers/sticky_headers.dart';

  SmartSelect<T>.single/multiple(
    ...,
    ...,
    groupBuilder: (context, header, choices) {
      return StickyHeader(
        header: header,
        content: choices,
      );
    },
  );
  ```

# Demo

## Preview

[![Watch the demo](https://img.youtube.com/vi/bcHELDM8hWg/maxresdefault.jpg)](https://youtu.be/bcHELDM8hWg)

## Download

[![Demo App](https://github.com/davigmacode/flutter_smart_select/raw/master/example/art/qr/apk.png "Demo App")](https://github.com/davigmacode/flutter_smart_select/blob/master/example/art/demo/SmartSelect.apk?raw=true)

# Features

* Select single or multiple choice
* Open choices modal in full page, bottom sheet, or popup dialog
* Various choices input (radio, checkbox, switch, chips, or custom widget)
* Various choices layout (list, wrap, or grid)
* Grouping choices with easy support to sticky header
* Searchable choices with highlighted result
* Disabled or hidden choices
* Customizable trigger/tile widget
* Customizable modal style
* Customizable modal header style
* Customizable modal footer
* Customizable choices style
* Flexible choices source type
* Easy async choices loader
* and many more

# Usage

For a complete usage, please see the [example](https://pub.dev/packages/smart_select#-example-tab-).

To read more about classes and other references used by `smart_select`, see the [API Reference](https://pub.dev/documentation/smart_select/4.0.0/).

## Single Choice

```dart
// available configuration for single choice
SmartSelect<T>.single({

  // The primary content of the widget.
  // Used in trigger widget and header option
  String title,

  // The text displayed when the value is null
  String placeholder = 'Select one',

  // The current value of the single choice widget.
  @required T value,

  // Called when single choice value changed
  @required ValueChanged<S2SingleState<T>> onChange,

  // choice item list
  List<S2Choice<T>> choiceItems,

  // other available configuration
  // explained below
  ...,
  ...,

})
```

```dart
// simple example

String value = 'flutter';
List<S2Choice<String>> options = [
  S2Choice<String>(value: 'ion', title: 'Ionic'),
  S2Choice<String>(value: 'flu', title: 'Flutter'),
  S2Choice<String>(value: 'rea', title: 'React Native'),
];

@override
Widget build(BuildContext context) {
  return SmartSelect<String>.single(
    title: 'Frameworks',
    value: value,
    choiceItems: options,
    onChange: (state) => setState(() => value = state.value)
  );
}
```

## Multiple Choice

```dart
SmartSelect<T>.multiple({

  // The primary content of the widget.
  // Used in trigger widget and header option
  String title,

  // The text displayed when the value is null
  String placeholder = 'Select one',

  // The current value of the single choice widget.
  @required List<T> value,

  // Called when single choice value changed
  @required ValueChanged<S2MultiState<T>> onChange,

  // choice item list
  List<S2Choice<T>> choiceItems,

  // other available configuration
  // explained below
  ...,
  ...,

})
```

```dart
// a simple example

List<int> value = [2];
List<S2Choice<int>> frameworks = [
  S2Choice<int>(value: 1, title: 'Ionic'),
  S2Choice<int>(value: 2, title: 'Flutter'),
  S2Choice<int>(value: 3, title: 'React Native'),
];

@override
Widget build(BuildContext context) {
  return SmartSelect<int>.multiple(
    title: 'Frameworks',
    value: value,
    choiceItems: options,
    onChange: (state) => setState(() => value = state.value),
  );
}
```

## Choices

```dart
// configuration
SmartSelect<T>.[single|multiple]({

  // other configuration
  ...,
  ...,

  // choice item list
  List<S2Choice<T>> choiceItems,

  // other configuration
  ...,
  ...,

});
```

`choiceItems` can be input directly as in the example below, more info about `S2Choice` can be found on the [API Reference](https://pub.dev/documentation/smart_select/latest/smart_select/S2Choice-class.html)

```dart
SmartSelect<T>.[single|multiple](
  ...,
  ...,
  choiceItems: <S2Choice<T>>[
    S2Choice<T>(value: 1, title: 'Ionic'),
    S2Choice<T>(value: 2, title: 'Flutter'),
    S2Choice<T>(value: 3, title: 'React Native'),
  ],
);
```

`choiceItems` also can be created from any list using helper provided by this package, like the example below

```dart
List<Map<String, String>> days = [
  { 'value': 'mon', 'title': 'Monday' },
  { 'value': 'tue', 'title': 'Tuesday' },
  { 'value': 'wed', 'title': 'Wednesday' },
  { 'value': 'thu', 'title': 'Thursday' },
  { 'value': 'fri', 'title': 'Friday' },
  { 'value': 'sat', 'title': 'Saturday' },
  { 'value': 'sun', 'title': 'Sunday' },
];

SmartSelect<String>.[single|multiple](
  ...,
  ...,
  choiceItems: S2Choice.listFrom<String, Map<String, String>>(
    source: days,
    value: (index, item) => item['value'],
    title: (index, item) => item['title'],
  ),
);
```

### Load Choice Item Asynchronously

Please follow these [example](https://github.com/davigmacode/flutter_smart_select/blob/master/example/lib/features_option/option_async.dart)

## Modal Configuration

```dart
// available configuration
SmartSelect<T>.[single|multiple]({

  // other configuration
  ...,
  ...,

  // Modal configuration
  S2ModalConfig modalConfig,

  // Configure modal style
  // shortcut to [modalConfig.style]
  S2ModalStyle modalStyle,

  // Configure modal header style
  // shortcut to [modalConfig.headerStyle]
  S2ModalHeaderStyle modalHeaderStyle,

  // Modal type to display choices
  // shortcut to [modalConfig.type]
  S2ModalType modalType,

  // Use different title with the trigger widget title
  // shortcut to [modalConfig.title]
  String modalTitle,

  // Whether the option list need to confirm
  // to return the changed value
  // shortcut to [modalConfig.useConfirmation]
  bool modalConfirmation,

  // Whether the options list modal use header or not
  // shortcut to [modalConfig.useHeader]
  bool modalHeader,

  // Whether the option list is filterable or not
  // shortcut to [modalConfig.useFilter]
  bool modalFilter,

  // Whether the filter is autocomplete or need confirmation
  // shortcut to [modalConfig.filterAuto]
  bool modalFilterAuto,

  // Custom searchbar hint
  // shortcut to [modalConfig.filterHint]
  String modalFilterHint,

  // other configuration
  ...,
  ...,

});
```

### Modal Type

By default SmartSelect will open choices modal in full page. You can change it by changing with this value:

```dart
// Target to open choices list
enum S2ModalType {

  // open in full page
  fullPage,

  // open in popup dialog
  popupDialog,

  // open in sliding bottom sheet
  bottomSheet,

}
```

### Modal Style

```dart
// Available option to configure modal style
S2ModalStyle({

  // Modal border shape
  // used in popup dialog and bottom sheet
  ShapeBorder shape,

  // Modal elevation
  // used in popup dialog and bottom sheet
  double elevation,

  // Modal background color
  Color backgroundColor,

  // Modal clip behavior
  Clip clipBehavior,

})
```

### Modal Header Style

```dart
// Available option to configure modal header style
S2ModalHeaderStyle({

  // Header border shape
  ShapeBorder shape,

  // Header elevation
  double elevation,

  // Header background color
  Color backgroundColor,

  // Header brightness
  Brightness brightness,

  // Whether the header title is centered
  bool centerTitle,

  // Whether the header use automaticallyImplyLeading or not
  bool useLeading,

  // Header text style
  // used by title and search field
  TextStyle textStyle,

  // Header icon theme
  IconThemeData iconTheme,

  // Header actions icon theme
  IconThemeData actionsIconTheme,

})
```

## Choices Configuration

```dart
// available choice type
// for single choice widget
SmartSelect<T>.[single|multiple]({

  // other configuration
  ...,
  ...,

  // choice configuration
  S2ChoiceConfig choiceConfig,

  // configure choice style
  // shortcut to [choiceConfig.style]
  S2ChoiceStyle choiceStyle,

  // configure choices group header style
  // shortcut to [choiceConfig.headerStyle]
  S2ChoiceHeaderStyle choiceHeaderStyle,

  // choice widget type
  // shortcut to [choiceConfig.type]
  S2ChoiceType choiceType,

  // choice layout to display items
  // shortcut to [choiceConfig.layout]
  S2ChoiceLayout choiceLayout,

  // choice list scroll direction
  // currently only support when
  // [layout] is [S2ChoiceLayout.wrap]
  // shortcut to [choiceConfig.direction]
  Axis choiceDirection,

  // Whether the choices list is grouped
  // shortcut to [choiceConfig.isGrouped]
  bool choiceGrouped,

  // Whether the choices item use divider or not
  // shortcut to [choiceConfig.useDivider]
  bool choiceDivider,

  // For grid choice layout
  // shortcut to [choiceConfig.gridDelegate]
  SliverGridDelegate choiceGrid,

  // other configuration
  ...,
  ...,

});
```

### Choice Type

By default SmartSelect will use `radios` for single choice and `checkboxes` for multiple choice, but it can change by changing  with this value:

```dart
// Type of choice input
enum S2ChoiceType {

  // use radio widget
  // for single choice
  radios,

  // use checkbox widget
  // for multiple choice
  checkboxes,

  // use switch widget
  // for multiple choice
  switches,

  // use chip widget
  // for single and multiple choice
  chips,

}
```
### Choice Layout

By default SmartSelect will use `list`, but it can change by changing  with this value:

```dart
// Layout of choice item
enum S2ChoiceLayout {

  // use list view widget
  list,

  // use wrap view widget
  wrap,

  // use grid view widget
  grid,

}
```

## Builder Widget

### Builder for Single Choice

```dart
// available builder configuration
// for single choice
SmartSelect<T>.single({

  // other configuration
  ...,
  ...,

  // Builder collection of single choice widget
  S2SingleBuilder<T> builder,

  // Builder for custom tile widget
  // shortcut to [builder.tileBuilder]
  S2WidgetBuilder<S2SingleState<T>> tileBuilder,

  // Builder for custom modal widget
  // shortcut to [builder.modalBuilder]
  S2WidgetBuilder<S2SingleState<T>> modalBuilder,

  // Builder for custom modal header widget
  // shortcut to [builder.modalHeaderBuilder]
  S2WidgetBuilder<S2SingleState<T>> modalHeaderBuilder,

  // Builder for custom modal confirm action widget
  // shortcut to [builder.modalConfirmBuilder]
  S2WidgetBuilder<S2SingleState<T>> modalConfirmBuilder,

  // Builder for divider widget between header, body, and footer modal
  // shortcut to [builder.modalDividerBuilder]
  S2WidgetBuilder<S2SingleState<T>> modalDividerBuilder,

  // Builder for custom footer widget
  // shortcut to [builder.modalFooterBuilder]
  S2WidgetBuilder<S2SingleState<T>> modalFooterBuilder,

  // other configuration
  ...,
  ...,

});
```

### Builder for Multiple Choice

```dart
// available builder configuration
// for multiple choice
SmartSelect<T>.multiple({

  // other configuration
  ...,
  ...,

  // Builder collection of single choice widget
  S2MultiBuilder<T> builder,

  // Builder for custom tile widget
  // shortcut to [builder.tileBuilder]
  S2WidgetBuilder<S2MultiState<T>> tileBuilder,

  // Builder for custom modal widget
  // shortcut to [builder.modalBuilder]
  S2WidgetBuilder<S2MultiState<T>> modalBuilder,

  // Builder for custom modal header widget
  // shortcut to [builder.modalHeaderBuilder]
  S2WidgetBuilder<S2MultiState<T>> modalHeaderBuilder,

  // Builder for custom modal confirm action widget
  // shortcut to [builder.modalConfirmBuilder]
  S2WidgetBuilder<S2MultiState<T>> modalConfirmBuilder,

  // Builder for divider widget between header, body, and footer modal
  // shortcut to [builder.modalDividerBuilder]
  S2WidgetBuilder<S2MultiState<T>> modalDividerBuilder,

  // Builder for custom footer widget
  // shortcut to [builder.modalFooterBuilder]
  S2WidgetBuilder<S2MultiState<T>> modalFooterBuilder,

  // other configuration
  ...,
  ...,

});
```

### Other Builder

```dart
// another builder configuration
SmartSelect<T>.[single|multiple]({

  // other configuration
  ...,
  ...,

  // Builder for modal filter widget
  // shortcut to [builder.modalFilterBuilder]
  S2WidgetBuilder<S2Filter> modalFilterBuilder,

  // Builder for modal filter toggle widget
  // shortcut to [builder.modalFilterToggleBuilder]
  S2WidgetBuilder<S2Filter> modalFilterToggleBuilder,

  // Builder for each custom choices item widget
  // shortcut to [builder.choiceBuilder]
  S2ChoiceBuilder<T> choiceBuilder,

  // Builder for each custom choices item title widget
  // shortcut to [builder.choiceTitleBuilder]
  S2ChoiceBuilder<T> choiceTitleBuilder,

  // Builder for each custom choices item subtitle widget
  // shortcut to [builder.choiceSubtitleBuilder]
  S2ChoiceBuilder<T> choiceSubtitleBuilder,

  // Builder for each custom choices item secondary widget
  // shortcut to [builder.choiceSecondaryBuilder]
  S2ChoiceBuilder<T> choiceSecondaryBuilder,

  /// Builder for custom divider widget between choices item
  // shortcut to [builder.choiceDividerBuilder]
  IndexedWidgetBuilder choiceDividerBuilder,

  // Builder for custom empty display
  // shortcut to [builder.choiceEmptyBuilder]
  S2WidgetBuilder<String> choiceEmptyBuilder,

  // A widget builder for custom choices group
  // shortcut to [builder.choiceGroupBuilder]
  S2ChoiceGroupBuilder choiceGroupBuilder,

  // A widget builder for custom header choices group
  // shortcut to [builder.choiceHeaderBuilder]
  S2ChoiceHeaderBuilder choiceHeaderBuilder,

  // Builder for choice items pager
  // shortcut to [builder.choicePagerBuilder]
  S2ChoiceBuilder<T> choicePagerBuilder,

  // Builder for progress indicator on choice load
  // shortcut to [builder.choiceProgressBuilder]
  WidgetBuilder choiceProgressBuilder,

  // other configuration
  ...,
  ...,

});
```

## Tile Widget

### Default Tile

```dart
// Default tile/trigger widget
S2Tile<T>({

  // The value of the selected option.
  String value,

  // The primary content of the list tile.
  Widget title,

  // A widget to display before the title.
  // Typically an [Icon] or a [CircleAvatar] widget.
  Widget leading,

  // A widget to display after the title.
  // Typically an [Icon] widget.
  Widget trailing,

  // Whether this list tile is intended to display loading stats.
  bool isLoading,

  // String text used as loading text
  String loadingText,

  // Whether this list tile is intended to display two lines of text.
  bool isTwoLine,

  // Whether this list tile is interactive.
  bool enabled,

  // If this tile is also [enabled] then icons and text are rendered with the same color.
  bool selected,

  // Whether this list tile is part of a vertically dense list.
  bool dense,

  // Whether the [value] is displayed or not
  bool hideValue,

  // The tile's internal padding.
  EdgeInsetsGeometry padding,

  // Called when the user taps this list tile.
  GestureTapCallback onTap,

})
```

```dart
// usage example
SmartSelect<T>.single(
  ...,
  ...,
  tileBuilder: (context, state) {
    return S2Tile(
      title: state.titleWidget,
      value: state.valueDisplay,
      onTap: state.showModal,
      isLoading: true,
    );
  },
);

// usage example from state
SmartSelect<T>.multiple(
  ...,
  ...,
  tileBuilder: (context, state) {
    return S2Tile.fromState(
      state,
      isLoading: true,
    );
  },
);
```

### Chips Tile

```dart
/// Chips tile/trigger widget
S2ChipsTile<T>({

  // List of value of the selected choices.
  List<S2Choice<T>> values,

  // Called when the user taps this list tile.
  GestureTapCallback onTap,

  // The primary content of the list tile.
  Widget title,

  // Additional content displayed below the title.
  Widget subtitle,

  // A widget to display before the title.
  Widget leading,

  // A widget to display after the title.
  Widget trailing,

  // Divider widget
  Widget divider,

  // The [Widget] displayed when the [values] is null
  Widget placeholder,

  // Hide placeholder when the [values] is null
  bool placeholderIgnored,

  // Whether the chip list is scrollable or not
  bool scrollable,

  // Chip list padding
  EdgeInsetsGeometry padding,

  // Chip color
  Color chipColor,

  // Chip border opacity
  double chipBorderOpacity,

  // Chip brightness
  Brightness chipBrightness,

  // Chip action button color
  Color chipActionColor,

  // Chip spacing
  double chipSpacing,

  // Chip run spacing
  double chipRunSpacing,

  // Widget builder for chip item
  IndexedWidgetBuilder chipBuilder,

  // Called when the user delete the chip item.
  ValueChanged<T> chipOnDelete,
})
```

```dart
/// usage example
SmartSelect<String>.multiple(
  ...,
  ...,
  value: cars,
  tileBuilder: (context, state) {
    return S2ChipsTile<String>(
      title: state.titleWidget,
      values: state.valueObject,
      onTap: state.showModal,
      subtitle: const Text('lorem impsum'),
      leading: const CircleAvatar(
        backgroundImage: NetworkImage('https://source.unsplash.com/8I-ht65iRww/100x100'),
      ),
      trailing: const Icon(Icons.add_circle_outline),
      scrollable: true,
      divider: const Divider(height: 1),
      chipColor: Colors.red,
      chipBrightness: Brightness.dark,
      onDeleted: (value) {
        setState(() => cars.remove(value));
      },
    );
  },
);

/// usage example from state
SmartSelect<String>.multiple(
  ...,
  ...,
  value: cars,
  tileBuilder: (context, state) {
    return S2ChipsTile<String>.fromState(
      state,
      subtitle: const Text('lorem impsum'),
      leading: const CircleAvatar(
        backgroundImage: NetworkImage('https://source.unsplash.com/8I-ht65iRww/100x100'),
      ),
      trailing: const Icon(Icons.add_circle_outline),
      scrollable: true,
      divider: const Divider(height: 1),
      chipColor: Colors.red,
      chipBrightness: Brightness.dark,
      onDeleted: (value) {
        setState(() => cars.remove(value));
      },
    );
  },
);
```

# License

```
Copyright (c) 2020 Irfan Vigma Taufik

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```