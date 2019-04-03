FsPdf
=====
_Builds PDF's from pure F# (and needs a more interesting name)_

Generates PDF content to enable developers to build PDF's programatically.  PDF's are helpful for offline documentation, 
printing, certificates of achievement, TPS reports, customer invoices, taxes, or being an actuary.

### What works?

* Makes a PDF without any extra dependencies (beyond a .NET Standard 2.0 runtime).
* Simple page layout formatting.
* Paths and shapes.
* Formatted text.
* A higher level DSL so you're not working with PDF primitives.
* Word wrap - wraps at spaces between words (left aligned text).

### What's in progress

* A nicer DSL for building text without having to understand PDF instructions.

### Then what?

* Font embedding.
* Images

### Example
Here is an example of a [PDF](https://gist.github.com/ninjarobot/550331efbe260f18a2a64352213af12b) generated by [this test](https://github.com/ninjarobot/FsPdf/blob/171a8d665f7b5a4cf1f80e6f2291d8e05a7b8a2b/tests/FsPdf.Tests/Tests.fs#L49).

### F# API

A `PdfFile` type is the main type to create, and it contains document metadata
as well as a "catalog" which holds the pages themselves.  The catalog has a
default page size that will apply to all pages unless they override it.  

Each `Page` holds `Resources` (currently just fonts), the contents as a list of
PDF stream instructions, and optionally can override the page media to have a
different page size.  Because the instructions themselves are low level, there
are some helpful functions for building shapes, wrapping strings, etc.

The result looks roughly like this:

```fsharp
let pdf =
    {
        Catalog =
            {
                PageLayout = SinglePage
                DefaultMedia = Media.Letter
                Pages =
                    [
                        {
                            Resources =
                                Map.empty
                                |> Map.add "F1" (FontResource (Type1, "Helvetica"))
                            Contents =
                                [
                                    BeginText
                                    Leading (20)
                                    FontSize ("F1", 12.)
                                    NextLineTranslate (50, 600)
                                    ShowText "hello world"
                                    EndText
                                ]
                            MediaSize = Some (Letter)
                        }
                    ]
            }
        Info = None
    }
```

This creates a PDF with a single page.  The page contains some text, set at 50
points from the left side and 600 points from the bottom of the page.
