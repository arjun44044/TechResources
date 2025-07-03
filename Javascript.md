# `padStart` and `Array.from()` Example----Project[Fitlab]

`getHourlySalesOfDay` controller--

```javascript
const mongoose = require('mongoose');
const Order = require('../models/orderModel');

const getHourlySalesOfDay = async (req, res) => {
  try {
    const { date } = req.query;

    if (!date) {
      return res.status(400).json({ message: 'Please provide a date in YYYY-MM-DD format.' });
    }

    const start = new Date(date);
    const end = new Date(date);
    end.setDate(end.getDate() + 1);

    const salesData = await Order.aggregate([
      {
        $match: {
          orderDate: { $gte: start, $lt: end },
          orderStatus: { $nin: ['cancelled', 'refunded'] }
        }
      },
      {
        $project: {
          hour: { $hour: '$orderDate' },
          total: '$absoluteTotalWithTaxes'
        }
      },
      {
        $group: {
          _id: '$hour',
          totalSales: { $sum: '$total' }
        }
      },
      {
        $project: {
          _id: 0,
          hour: '$_id',
          totalSales: 1
        }
      }
    ]);

    // Fill in missing hours with zero sales
    const fullDay = Array.from({ length: 24 }, (_, i) => {
      const found = salesData.find(s => s.hour === i);
      return {
        hour: `${i.toString().padStart(2, '0')}:00`,
        totalSales: found ? found.totalSales : 0
      };
    });

    res.status(200).json(fullDay);
  } catch (err) {
    console.error('Hourly sales error:', err);
    res.status(500).json({ message: 'Server error while fetching hourly sales' });
  }
};

module.exports = { getHourlySalesOfDay };

```

Here---

```javascript
const fullDay = Array.from({ length: 24 }, (_, i) => {
  const found = salesData.find(s => s.hour === i);
  return {
    hour: `${i.toString().padStart(2, '0')}:00`,
    totalSales: found ? found.totalSales : 0
  };
});

```

### 🔍 Detailed Breakdown

#### ✅ `Array.from({ length: 24 }, (_, i) => { ... })`

This creates an array of  **24 elements** , from `i = 0` to `23` — representing **0 to 23 hours** in a day.

* `i` is the hour index (0 to 23).
* `_` is the unused first argument (value), which is ignored.

#### ✅ `const found = salesData.find(s => s.hour === i);`

This checks if there is an entry in `salesData` for the current hour (`i`).

* `salesData` is likely the result of a MongoDB aggregation where each object is:

`{ hour: 13, totalSales: 4800 }`

✅ `hour: \`$:00``

Formats the hour like `"00:00"`, `"01:00"`, ..., `"23:00"` using:

* `toString()` – converts number to string
* `padStart(2, '0')` – ensures it's always two digits (e.g., `1` → `"01"`)

#### ✅ `totalSales: found ? found.totalSales : 0`

* If there is a `salesData` entry for that hour, it uses the value.
* Otherwise, it sets `totalSales` to `0`.

If `salesData` looks like this:

```javascript
[
  { hour: 0, totalSales: 500 },
  { hour: 13, totalSales: 2450 },
  { hour: 22, totalSales: 800 }
]

```

Then `fullDay` becomes:

```javascript
[
  { hour: "00:00", totalSales: 500 },
  { hour: "01:00", totalSales: 0 },
  ...
  { hour: "13:00", totalSales: 2450 },
  ...
  { hour: "22:00", totalSales: 800 },
  { hour: "23:00", totalSales: 0 }
]

```

### ✅ What is `padStart(2, '0')`?

`padStart(targetLength, padChar)` is a **JavaScript string method** that:

* Pads the beginning of a string with a character (like `'0'`)
* Until the string reaches the desired length (`2` in this case)

### ✨ Example 1: Single-digit number

Let’s say the number is `5`:

```javascript
const i = 5;
const padded = i.toString().padStart(2, '0');
console.log(padded); // "05"

```

Here's what happens:

* `i.toString()` gives `"5"`
* `.padStart(2, '0')` adds a `'0'` in front to make it `"05"`

-- Now say the number is `17`: `.padStart(2, '0')`  **does nothing** , because it's already 2 characters long — so the result is still `"17"`

# DATES--More Details

```javascript
const start = new Date(date)
const end = new Date(date)
end.setDate(end.getDate() + 1)

```

###### 🔍 Let's break it down:

--`const start = new Date(date)`

* This converts the given `date` (usually a string like `"2025-04-29"`) into a JavaScript `Date` object.
* For example:

`const date = '2025-04-29';`

` const start = new Date(date); // => Tue Apr 29 2025 00:00:00 GMT...`

-- `end.setDate(end.getDate() + 1)`

* This adds **1 day** to the `end` date.
* So if `end` was initially `2025-04-29`, it now becomes `2025-04-30`.

🕒 Final Result:

* `start` → `2025-04-29T00:00:00.000Z`
* `end` → `2025-04-30T00:00:00.000Z`

This allows you for example to query documents **within one full day** by filtering:  `{ orderDate: { $gte: start, $lt: end } }--> Everything on 2025-04-29 from 00:00:00.000 up to (but not including) 2025-04-30T00:00:00.000.`

### **`2025-04-30T00:00:00.000 (ISO DATE FORMATS)`**

This is an **ISO 8601** date-time string — a standard format for representing date and time, often used in databases, APIs, and logs.

📘 Components of `2025-04-30T00:00:00.000`

| Part     | Value          | Meaning                         |
| -------- | -------------- | ------------------------------- |
| `2025` | Year           | The year is 2025                |
| `04`   | Month          | April (months are 1-based here) |
| `30`   | Day            | The 30th day of the month       |
| `T`    | Time separator | Indicates that the time follows |
| `00`   | Hours          | 00 hours (midnight)             |
| `00`   | Minutes        | 00 minutes                      |
| `00`   | Seconds        | 00 seconds                      |
| `.000` | Milliseconds   | 000 milliseconds                |

So, this represents:

> 🕛 Midnight at the **very start** of April 30, 2025.

* The `Z` stands for  **Zulu Time** , aka  **UTC (Coordinated Universal Time)** .
* If you're in a timezone like IST (UTC+5:30),  I**ndian Standard Time (IST), which equates to ****UTC+05:30**, i.e. five and a half hours ahead of Coordinated Universal Time
* **Coordinated Universal Time** ( **UTC** ) is the primary [time standard](https://en.wikipedia.org/wiki/Time_standard "Time standard") globally used to regulate clocks and time. It establishes a reference for the current time, forming the basis for [civil time](https://en.wikipedia.org/wiki/Civil_time "Civil time") and [time zones](https://en.wikipedia.org/wiki/Time_zones "Time zones"). UTC facilitates international communication, navigation, scientific research, and commerce.
* It **is the standard time common to every place in the world. It is also known as Greenwich Mean Time (GMT) and Zulu (Z).**

**Hence `2025-04-30T00:00:00.000`  would be  `2025-04-30T05:30:00.000+05:30`**

# `salesDate instanceof Date ? salesDate.toISOString().split('T')[0] : ''`

```javascript
const [salesDate, setSalesDate] = useState(new Date())
The jsx part--
{
                  salesDate &&
                  <input type="date" id="date" name="date" value={salesDate} onChange={(e)=> setSalesDate(e.target.value)}
                    className="w-[8.5rem] h-[2.1rem] text-[11px] text-secondary capitalize border border-primary rounded-[6px]
                     cursor-pointer focus:ring-2 focus:ring-primaryDark focus:outline-none"/>
     }
```

Its working fine but by default on the initial render, it doesn't show the current date instead shows 'mm/dd/yyy'

> The issue you're seeing is because you're initializing `salesDate` with a `Date` object, but you're using it as a `value` in an `<input type="date">`, which expects a  **string in the format `yyyy-mm-dd `**  (Not 'mm/dd/yyy' browser showin that depending on locale), not a `Date` object.
>
> Hence convert it to format `yyyy-mm-dd`
>
> Because:
>
> * `salesDate.toISOString()` → `"2025-05-01T10:30:00.000Z"`
> * `.split('T')[0]` → `"2025-05-01"` ✅ valid for `<input type="date">`

Hence---

```javascript
const [salesDate, setSalesDate] = useState(new Date())
//The jsx part---
{
                  salesDate &&
                  <input type="date" id="date" name="date" value={salesDate instanceof Date ? salesDate.toISOString().split('T')[0] : salesDate}
                   onChange={(e)=> setSalesDate(e.target.value)}
                    className="w-[8.5rem] h-[2.1rem] text-[11px] text-secondary capitalize border border-primary rounded-[6px]
                     cursor-pointer focus:ring-2 focus:ring-primaryDark focus:outline-none"/>
                }

// OR---

const [salesDate, setSalesDate] = useState(() => new Date().toISOString().split('T')[0]);

<input
  type="date"
  value={salesDate}
  onChange={(e) => setSalesDate(e.target.value)}
/>

```

# Converting Object into Array of arrays

```javascript
const categoryData = Object.entries(categoryRevenueMap).map(([name, revenue]) => ({
  name,
  revenue: Math.round(revenue * 100) / 100  // round to 2 decimals
}));

```

This code transforms an object (`categoryRevenueMap`) into an array of objects with `name` and `revenue` properties. The `revenue` is rounded to  **2 decimal places** .

### 🧠 Step-by-Step Breakdown:

#### 1. **`Object.entries(categoryRevenueMap)`**

* Converts an object into an array of `[key, value]` pairs.
* Example:

```javascript
const categoryRevenueMap = {
  Cardio: 1254.565,
  Strength: 899.2345,
  Accessories: 450
};
Object.entries(categoryRevenueMap);
// Output: [ ['Cardio', 1254.565], ['Strength', 899.2345], ['Accessories', 450] ]

```

#### 2. **`.map(([name, revenue]) => { ... })`**

* Destructures each pair:
  * `name` is the category name (e.g., "Cardio")
  * `revenue` is the value (e.g., 1254.565)
* It returns a new object for each entry.
* > **Understand that the first parameter of .map() is the item or in this case an array because we are dealing with Array of arrays which is simultaneously destrutured as `map` and **`b`
  >

#### 3. **Rounding Revenue:**

`Math.round(revenue * 100) / 100`

Rounds the revenue to 2 decimal places:

* `1254.565 → 125457 → 1255 → 12.55`

### ✅ Final Output:

You get an array like:

```javascript
[
  { name: 'Cardio', revenue: 1254.57 },
  { name: 'Strength', revenue: 899.23 },
  { name: 'Accessories', revenue: 450.00 }
]


```

# Date- More Examples

getCouponRevenueStats() is a controller for total coupon revenue calculation to **include all coupon discounts from the beginning until yesterday** (excluding today) and computes the **percentage increase or decrease comparing yesterday's and today's**

```javascript
const getCouponRevenueStats = async(req, res, next)=> {
  const now = new Date()

  const startOfToday = new Date(now.setHours(0, 0, 0, 0))
  const endOfToday = new Date(now.setHours(23, 59, 59, 999))

  const startOfYesterday = new Date(startOfToday)
  startOfYesterday.setDate(startOfYesterday.getDate() - 1)
  const endOfYesterday = new Date(startOfYesterday)
  endOfYesterday.setHours(23, 59, 59, 999)

  const totalCouponRevenueData = await Order.aggregate([
    {
      $match: {
        couponUsed: { $ne: null },
        createdAt: { $lt: startOfToday } 
      }
    },
    {
      $group: {
        _id: null,
        totalDiscount: { $sum: "$couponDiscount" }
      }
    }
  ])

  const todayData = await Order.aggregate([
    {
      $match: {
        couponUsed: { $ne: null },
        createdAt: { $gte: startOfToday, $lte: endOfToday }
      }
    },
    {
      $group: {
        _id: null,
        totalDiscount: { $sum: "$couponDiscount" }
      }
    }
  ])

  const yesterdayData = await Order.aggregate([
    {
      $match: {
        couponUsed: { $ne: null },
        createdAt: { $gte: startOfYesterday, $lte: endOfYesterday }
      }
    },
    {
      $group: {
        _id: null,
        totalDiscount: { $sum: "$couponDiscount" }
      }
    }
  ])

  const totalCouponRevenue = totalCouponRevenueData[0]?.totalDiscount || 0
  const todayRevenue = todayData[0]?.totalDiscount || 0
  const yesterdayRevenue = yesterdayData[0]?.totalDiscount || 0

  let percentageChange = 0
  if (yesterdayRevenue !== 0) {
    percentageChange = ((todayRevenue - yesterdayRevenue) / yesterdayRevenue) * 100
  } else if (todayRevenue !== 0) {
    percentageChange = 100
  }

  res.status(200).json({
    totalCouponRevenue,
    todayRevenue,
    yesterdayRevenue,
    percentageChange: Math.round(percentageChange * 100) / 100
  });
}


```

Example 2--

```javascript
const getOfferStats = async (req, res, next) => {
  try {
    console.log("getOfferStats-->", getOfferStats)
    const today = new Date()
    today.setHours(0, 0, 0, 0)

    const startOfMonth = new Date(today.getFullYear(), today.getMonth(), 1)
    const endOfMonth = new Date(today.getFullYear(), today.getMonth() + 1, 0, 23, 59, 59, 999)

    const activeOffersCount = await Product.countDocuments({
      "offerApplied": { $ne: null },
      "offerApplied.offerEndDate": { $gte: today },
    })
    console.log("activeOffersCount-->", activeOffersCount)

    const expiredOffersCount = await Product.countDocuments({
      "offerApplied": { $ne: null },
      "offerApplied.offerEndDate": {
        $gte: startOfMonth,
        $lte: today, 
      },
    })
    console.log("expiredOffersCount-->", expiredOffersCount)

    return res.status(200).json({
      activeOffersCount,
      expiredOffers: expiredOffersCount,
    });
  }
  catch(error){
    console.error("Error in getOfferStatusStats:", error.message)
    next(error)
  }
}
```

# Navigator and Navigator.mediaDevices()

### 🌐Navigator.mediaDevices()

`navigator.mediaDevices` is part of the **Media Capture and Streams API** (aka  **MediaDevices API** ). It provides access to connected media input devices such as cameras, microphones, and screen sharing.

It is a **property of the `navigator` object** (which represents the browser's user agent). It gives you:

* Methods to access hardware (camera, mic, screen)
* Promises-based APIs
* Ability to enumerate devices

##### 🔧 Properties & Methods of `navigator.mediaDevices`

| Method / Property             | Description                                                                          |
| ----------------------------- | ------------------------------------------------------------------------------------ |
| `getUserMedia(constraints)` | Prompts the user for access to their camera/microphone.                              |
| `enumerateDevices()`        | Lists all connected media input/output devices.                                      |
| `getDisplayMedia()`         | Prompts the user to share their screen (used in screen sharing).                     |
| `ondevicechange`            | Event handler triggered when media devices change (e.g. a new webcam is plugged in). |

##### 📸 What is `navigator.mediaDevices.getUserMedia()`?

This method prompts the user for permission to use **input devices** such as:

* **Webcam (video input)**
* **Microphone (audio input)**

And returns a **`Promise`** that resolves with a `MediaStream` object if the user grants permission.

##### ✅ Syntax:

```javascript
navigator.mediaDevices.getUserMedia(constraints)
  .then(function(mediaStream) {
    // Use the mediaStream (e.g. display video)
  })
  .catch(function(err) {
    // Handle the error
  });

```

**🔒 Constraints**

You pass a **constraints object** to tell the browser what you want:

Example 1: Audio and Video --- `{ audio: true, video: true }`

Example 2: High-resolution front camera

```javascript
{
  video: {
    width: { ideal: 1280 },
    height: { ideal: 720 },
    facingMode: "user" // "user" for front camera, "environment" for rear
  },
  audio: true
}

```

**📽️ Basic Usage Example**

```html
<video id="myVideo" autoplay playsinline></video>

```

```javascript
const video = document.getElementById('myVideo');

navigator.mediaDevices.getUserMedia({ video: true, audio: true })
  .then((stream) => {
    video.srcObject = stream;
  })
  .catch((error) => {
    console.error('Error accessing media devices.', error);
  });

```

**📦 MediaStream**

The returned `MediaStream` contains one or more **`MediaStreamTrack`** objects (video or audio tracks), accessible via:

```javascript
stream.getTracks(); → returns all media tracks (audio + video)
stream.getVideoTracks();  → only video tracks
stream.getAudioTracks();  → only audio tracks
```

##### 🛑 Error Handling

If the user denies access or no devices are available, the `Promise` is rejected.

**Common Errors:**

* `NotAllowedError` – User denied permission.
* `NotFoundError` – No device found matching constraints.
* `NotReadableError` – Hardware is already in use.
* `OverconstrainedError` – Constraints could not be satisfied.

### 🧪 `enumerateDevices()`

EXAMPLE-- To list all connected cameras, microphones, and speakers:

```javascript
navigator.mediaDevices.enumerateDevices().then(devices => {
  devices.forEach(device => {
    console.log(`${device.kind}: ${device.label} id=${device.deviceId}`);
  });
});

```

**🔹 `device.kind`**

* **Type** : `string,`   **Purpose** : Specifies the kind of device.
* **Possible values:**

  * `'audioinput'` → Microphones
  * `'audiooutput'` → Speakers/headphones
  * `'videoinput'` → Cameras

**🔹 `device.label`**

* **Type** : `string`
* **Purpose** : Provides a human-readable label (e.g., "HD Webcam" or "Built-in Microphone").

> ⚠️  **Important** : This is only available if the user has **granted media permissions** (e.g., via `getUserMedia`). Otherwise, it will be an empty string for privacy reasons.

**🔹 `device.deviceId`**

* **Type** : `string`
* **Purpose** : A unique identifier for the device (not guaranteed to be permanent or user-trackable).
* You use this `deviceId` to select a specific device when calling `getUserMedia`, like:

### **🖥️ Screen Sharing (via `getDisplayMedia()`)**

```
navigator.mediaDevices.getDisplayMedia({ video: true })
  .then(stream => {
    document.querySelector('video').srcObject = stream;
  });

```

**🔁 Releasing the Media Stream**

When done, you should stop the stream to release the camera/mic:

`stream.getTracks().forEach(track => track.stop());`

### 🔍 `ondevicechange`

The `ondevicechange` event handler is triggered when the list of **media input/output devices changes** — for example:

* A **new device is connected** (e.g., webcam, microphone, speaker).
* A device is  **disconnected** .
* The user **plugs in or unplugs** a headset, webcam, or microphone.

It is part of the **MediaDevices API** and is useful for building responsive applications that adapt to changing hardware.

**✅ Basic Usage**

```javascript
navigator.mediaDevices.ondevicechange = () => {
  console.log("Media devices changed");

  navigator.mediaDevices.enumerateDevices()
    .then(devices => {
      devices.forEach(device => {
        console.log(`${device.kind}: ${device.label}`);
      });
    });
};

```

**🚨 ImpoHrtant Notes**

* It  **does not provide device details directly** . You must use `enumerateDevices()` again to get the updated list.
* Browsers **may not fire this event immediately** — some implementations debounce it or group events.
* User permission is **not required** just to listen for the event, but  **device labels will be empty unless permission was previously granted** .
