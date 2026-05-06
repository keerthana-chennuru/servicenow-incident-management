# 🎫 ServiceNow Incident Management Enhancements

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1000&color=6A9FBF&width=600&lines=Incident+Management+Enhancements;UI+Actions+%7C+Client+Scripts+%7C+UI+Policies;ITSM+Configuration+%26+Scripting" alt="Typing SVG" />

![Platform](https://img.shields.io/badge/Platform-ServiceNow-green?style=for-the-badge&logo=servicenow)
![Scope](https://img.shields.io/badge/Scope-POC%20ITSM%20Management-blue?style=for-the-badge)
![Type](https://img.shields.io/badge/Type-ITSM%20Configuration%20%26%20Scripting-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

---

## 📌 Overview

Enhanced the **Incident Management** module in ServiceNow by implementing real-world business requirements focused on usability, process governance, and ITSM compliance. The enhancements span **UI Actions**, **Client Scripts**, **UI Policies**, and **Dictionary** configurations.

---

## 🎯 Objectives

- ✅ Add a direct link from Incident form to create a related Change Request
- ✅ Build a dedicated Closure Information section with mandatory fields
- ✅ Auto-populate the "Closed By" field when an incident is closed
- ✅ Improve user guidance through help icons on key fields
- ✅ Enhance Assignment Group selection using a tree structure
- ✅ Enforce process compliance rules (mandatory work notes, closure fields)

---

## 🏗️ Enhancements Implemented

### 1. Change Request Link via UI Action

A Related Link **"ChangeRequestLink"** was added to the Incident form:

| Property | Value |
|---|---|
| Name | `ChangeRequestLink` |
| Action name | `change_request_link` |
| Table | POC Incident table |
| Type | Form link (not a button) |
| Visible on | Insert and Update |

```javascript
// UI Action Script
action.setRedirectURL('u_poc_change_table_list.do');
```

---

### 2. Incident Closure Information Section

A dedicated **"Closure Information"** section was added to the Incident form containing:

| Field | Type | Purpose |
|---|---|---|
| Closed Code | Dropdown | Standardised closure reasons |
| Closed Notes | Free text | Resolution summary |
| Closed By | Reference field | Auto-populated via Client Script |

---

### 3. Auto-Populate "Closed By" (Client Script)

When `State` is changed to **Closed**, the "Closed By" field is automatically populated with the currently logged-in user.

```javascript
// Client Script: "Closed by" | Type: onChange | Field: State
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading || newValue === '') {
        return;
    }
    if (newValue == '7') {
        g_form.setValue('u_closed_by', g_user.userID);
    }
}
```

> ✅ When `State = Closed (value 7)` → `Closed By = current logged-in user` automatically.

---

### 4. Mandatory Closure Fields (UI Policy)

| Property | Value |
|---|---|
| Condition | State is Closed |
| Mandatory fields | `u_closed_notes`, `u_choice_2` (Closed Code) |
| Reverse if false | Enabled |

---

### 5. Help Icons for Urgency & Impact (Client Script)

```javascript
function onLoad() {
    g_form.addDecoration('impact', 'icon-help',
        'Defines how many users or business services are affected by the incident.');
    g_form.addDecoration('urgency', 'icon-help',
        'Defines how quickly the incident must be resolved based on business need.');
}
```

---

### 6. Assignment Group Tree Structure

| Property | Value |
|---|---|
| Field | `assignment_group` on Incident table |
| Attribute Added | `tree_picker=true` |
| Method | Dictionary Override on Incident table |

---

### 7. Mandatory Work Notes on Assignment Change (UI Policy)

| Property | Value |
|---|---|
| Condition | Assignment group is not empty |
| Field made mandatory | `work_notes = True` |

---

## ⚙️ Technical Summary

| Enhancement | Type | Script Used |
|---|---|---|
| Change Request Link | UI Action | `action.setRedirectURL()` |
| Auto Closed By | Client Script (onChange) | `g_form.setValue()`, `g_user.userID` |
| Mandatory Closure Fields | UI Policy | Condition-based |
| Help Icons | Client Script (onLoad) | `g_form.addDecoration()` |
| Tree Picker for Groups | Dictionary Override | `tree_picker=true` attribute |
| Mandatory Work Notes | UI Policy | Condition-based |

---

## ✅ Skills Demonstrated

![UI Actions](https://img.shields.io/badge/UI-Actions-green?style=flat-square)
![Client Scripts](https://img.shields.io/badge/Client-Scripts-blue?style=flat-square)
![UI Policies](https://img.shields.io/badge/UI-Policies-orange?style=flat-square)
![Dictionary](https://img.shields.io/badge/Dictionary-Overrides-purple?style=flat-square)

- 🔗 UI Actions (Form Link type)
- 📝 Client Scripts (`onChange`, `onLoad`)
- 📋 UI Policies and UI Policy Actions
- 📖 Dictionary Entries and Dictionary Overrides
- 🔧 `g_form` API (`setValue`, `addDecoration`)
- 👤 `g_user` API (`userID`)
- 🎨 ServiceNow form design and section management
- ✅ ITSM process compliance implementation

---

## 🎯 Outcome

| Area | Improvement |
|---|---|
| Governance | Closure fields enforced before incidents can be closed |
| Documentation | Mandatory work notes on assignment changes |
| User Experience | Help icons guide agents on field selection |
| Navigation | Tree structure for Assignment Group |
| Process Integration | Direct link from Incident to Change Request |

---

## ⚠️ Common Mistakes to Avoid

| ❌ Mistake | ✅ Best Practice |
|---|---|
| Using wrong field name in `g_form.setValue()` | Always use the exact system field name |
| Forgetting `Reverse if false` on UI Policy | Enable it so fields reset when condition changes |
| Not checking `isLoading` in onChange scripts | Always return early if `isLoading` is true |
| Setting `tree_picker` on wrong dictionary level | Use Dictionary Override on the specific table |



---

<div align="center">

**Made with ❤️ by Keerthana Chennuru**

![ServiceNow](https://img.shields.io/badge/Built%20on-ServiceNow-green?style=for-the-badge)

</div>
