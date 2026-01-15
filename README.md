# Obsidian Hijri Template

Automatic Hijri (Islamic calendar) dates for Obsidian journal entries using Templater plugin.

## Features

- Automatically names new files with Hijri date (e.g., `26-Rajab-1447.md`)
- Adds frontmatter with Hijri date and creation timestamp
- Works with folder templates - creates entries in the correct format when you create a file in your journal folder

## Requirements

- [Obsidian](https://obsidian.md/)
- [Templater plugin](https://github.com/SilentVoid13/Templater)
- Python 3 with `hijridate` package

## Installation

### 1. Install hijridate Python package

```bash
pip install hijridate
```

### 2. Create the hijri-today script

Create a file `~/bin/hijri-today` (or any location you prefer):

```python
#!/usr/bin/python3
import sys
sys.path.insert(0, '/path/to/your/site-packages')  # Update this path!
from hijridate import Gregorian
h = Gregorian.today().to_hijri()
m = {1:'Muharram',2:'Safar',3:'Rabi-al-Awwal',4:'Rabi-al-Thani',5:'Jumada-al-Awwal',6:'Jumada-al-Thani',7:'Rajab',8:'Shaban',9:'Ramadan',10:'Shawwal',11:'Dhul-Qadah',12:'Dhul-Hijjah'}
print(f'{h.day:02d}-{m[h.month]}-{h.year}')
```

Find your site-packages path:
```bash
python3 -c "import hijridate; print(hijridate.__file__)"
```

Make it executable:
```bash
chmod +x ~/bin/hijri-today
```

Test it:
```bash
~/bin/hijri-today
# Output: 26-Rajab-1447
```

### 3. Configure Templater

1. Open Obsidian Settings → Templater
2. Enable **"Trigger Templater on new file creation"**
3. Enable **"Enable user system command functions"**
4. Add User Function:
   - Function name: `hijri`
   - System command: `/full/path/to/hijri-today`
5. Add Folder Template:
   - Folder: your journal folder path
   - Template: `Templates/Hijri.md`

### 4. Create the template

Create `Templates/Hijri.md` in your vault:

```markdown
<%*
const hijri = await tp.user.hijri();
await tp.file.rename(hijri.trim());
-%>
---
date: <% hijri %>
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
---

# <% hijri %>

```

## Usage

Simply create a new file in your configured journal folder. The template will:
1. Automatically rename the file to the current Hijri date
2. Add frontmatter with the date
3. Create a heading with the date

## Folder Structure Example

```
Journal/
└── 1447/
    ├── 06-Jumada-al-Thani/
    │   └── 17-Jumada-al-Thani-1447.md
    └── 07-Rajab/
        ├── 02-Rajab-1447.md
        ├── 10-Rajab-1447.md
        └── 26-Rajab-1447.md
```

## Hijri Months Reference

| # | Name | Arabic |
|---|------|--------|
| 1 | Muharram | محرم |
| 2 | Safar | صفر |
| 3 | Rabi-al-Awwal | ربيع الأول |
| 4 | Rabi-al-Thani | ربيع الآخر |
| 5 | Jumada-al-Awwal | جمادى الأولى |
| 6 | Jumada-al-Thani | جمادى الآخرة |
| 7 | Rajab | رجب |
| 8 | Shaban | شعبان |
| 9 | Ramadan | رمضان |
| 10 | Shawwal | شوال |
| 11 | Dhul-Qadah | ذو القعدة |
| 12 | Dhul-Hijjah | ذو الحجة |

## License

MIT
