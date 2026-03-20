# Advanced Dashboard & Data Visualization Patterns

> Inspired by modern SaaS dashboards: Spark Pixel, Kravio, and enterprise analytics interfaces

---

## Dashboard Layout Patterns

### 1. Sidebar Navigation

**Modern sidebar design principles:**

```css
.sidebar {
  width: 280px;
  background: var(--color-bg-primary);
  border-right: 1px solid var(--color-border-light);
  padding: var(--space-6);
  display: flex;
  flex-direction: column;
  gap: var(--space-8);
}

/* Company/User Header */
.sidebar-header {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-4);
  border-radius: var(--radius-lg);
  background: var(--color-bg-secondary);
}

.company-avatar {
  width: 40px;
  height: 40px;
  border-radius: var(--radius-md);
  background: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: var(--font-semibold);
}

/* Navigation Groups */
.nav-group {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
}

.nav-group-label {
  font-size: var(--text-xs);
  font-weight: var(--font-semibold);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--color-text-tertiary);
  padding: var(--space-2) var(--space-3);
  margin-bottom: var(--space-2);
}

/* Navigation Items */
.nav-item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-3);
  border-radius: var(--radius-md);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  transition: all 0.2s ease;
  cursor: pointer;
}

.nav-item:hover {
  background: var(--color-bg-secondary);
  color: var(--color-text-primary);
}

.nav-item.active {
  background: var(--color-bg-secondary);
  color: var(--color-text-primary);
  font-weight: var(--font-medium);
}

.nav-item-icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}
```

### 2. Stats Cards (KPI Widgets)

**Clean, scannable metrics:**

```tsx
interface StatCardProps {
  label: string;
  value: string | number;
  change?: {
    value: number;
    period: string;
  };
  trend?: 'up' | 'down';
  icon?: React.ReactNode;
}

function StatCard({ label, value, change, trend, icon }: StatCardProps) {
  return (
    <div className="stat-card">
      <div className="stat-header">
        <span className="stat-label">{label}</span>
        {icon && <div className="stat-icon">{icon}</div>}
      </div>
      
      <div className="stat-value">{value}</div>
      
      {change && (
        <div className={`stat-change ${trend}`}>
          <span className="change-value">
            {change.value > 0 ? '+' : ''}{change.value}%
          </span>
          <span className="change-period">{change.period}</span>
        </div>
      )}
    </div>
  );
}
```

```css
.stat-card {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-xl);
  padding: var(--space-6);
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
}

.stat-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.stat-label {
  font-size: var(--text-xs);
  font-weight: var(--font-medium);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--color-text-tertiary);
}

.stat-value {
  font-size: var(--text-h2);
  font-weight: var(--font-semibold);
  color: var(--color-text-primary);
  line-height: 1;
}

.stat-change {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  font-size: var(--text-sm);
}

.stat-change.up .change-value {
  color: var(--color-success);
}

.stat-change.down .change-value {
  color: var(--color-error);
}

.change-period {
  color: var(--color-text-tertiary);
}
```

### 3. Grid Layouts (Stats Overview)

```css
/* 4-column grid for metrics */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--space-6);
  margin-bottom: var(--space-8);
}

/* Responsive breakpoints */
@media (max-width: 1280px) {
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .stats-grid {
    grid-template-columns: 1fr;
  }
}
```

---

## Data Visualization Components

### 1. Bar Charts (Sparkline Style)

```tsx
function BarChart({ data, height = 200 }: { data: number[]; height?: number }) {
  const max = Math.max(...data);
  
  return (
    <div className="bar-chart" style={{ height }}>
      {data.map((value, index) => (
        <div
          key={index}
          className="bar"
          style={{
            height: `${(value / max) * 100}%`,
            opacity: value === max ? 1 : 0.7,
          }}
        />
      ))}
    </div>
  );
}
```

```css
.bar-chart {
  display: flex;
  align-items: flex-end;
  gap: 2px;
  padding: var(--space-4);
}

.bar {
  flex: 1;
  background: var(--color-primary);
  border-radius: 2px 2px 0 0;
  transition: all 0.3s ease;
}

.bar:hover {
  opacity: 1 !important;
  background: var(--color-accent);
}
```

### 2. Line Charts (Trend Indicators)

```tsx
function MiniLineChart({ data, color }: { data: number[]; color: string }) {
  return (
    <svg className="mini-line-chart" viewBox="0 0 100 30">
      <polyline
        points={data.map((val, i) => `${(i / (data.length - 1)) * 100},${30 - (val / Math.max(...data)) * 25}`).join(' ')}
        fill="none"
        stroke={color}
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      />
    </svg>
  );
}
```

```css
.mini-line-chart {
  width: 100px;
  height: 30px;
}
```

### 3. Donut/Circular Progress

```tsx
function CircularProgress({ percentage, size = 120 }: { percentage: number; size?: number }) {
  const strokeWidth = 8;
  const radius = (size - strokeWidth) / 2;
  const circumference = 2 * Math.PI * radius;
  const offset = circumference - (percentage / 100) * circumference;

  return (
    <svg width={size} height={size} className="circular-progress">
      {/* Background circle */}
      <circle
        cx={size / 2}
        cy={size / 2}
        r={radius}
        fill="none"
        stroke="var(--color-bg-tertiary)"
        strokeWidth={strokeWidth}
      />
      {/* Progress circle */}
      <circle
        cx={size / 2}
        cy={size / 2}
        r={radius}
        fill="none"
        stroke="var(--color-primary)"
        strokeWidth={strokeWidth}
        strokeDasharray={circumference}
        strokeDashoffset={offset}
        strokeLinecap="round"
        transform={`rotate(-90 ${size / 2} ${size / 2})`}
        style={{ transition: 'stroke-dashoffset 0.5s ease' }}
      />
      {/* Percentage text */}
      <text
        x="50%"
        y="50%"
        textAnchor="middle"
        dy=".3em"
        fontSize="24"
        fontWeight="600"
        fill="var(--color-text-primary)"
      >
        {percentage}%
      </text>
    </svg>
  );
}
```

---

## Table Patterns (Data Tables)

### Clean, Scannable Tables

```tsx
interface Column<T> {
  key: keyof T;
  header: string;
  width?: string;
  render?: (value: any, row: T) => React.ReactNode;
}

function DataTable<T>({ data, columns }: { data: T[]; columns: Column<T>[] }) {
  return (
    <div className="data-table-container">
      <table className="data-table">
        <thead>
          <tr>
            {columns.map((col) => (
              <th key={String(col.key)} style={{ width: col.width }}>
                {col.header}
              </th>
            ))}
          </tr>
        </thead>
        <tbody>
          {data.map((row, rowIndex) => (
            <tr key={rowIndex}>
              {columns.map((col) => (
                <td key={String(col.key)}>
                  {col.render ? col.render(row[col.key], row) : String(row[col.key])}
                </td>
              ))}
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
```

```css
.data-table-container {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-xl);
  overflow: hidden;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
}

.data-table thead {
  background: var(--color-bg-secondary);
  border-bottom: 1px solid var(--color-border-light);
}

.data-table th {
  padding: var(--space-3) var(--space-4);
  text-align: left;
  font-size: var(--text-xs);
  font-weight: var(--font-semibold);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--color-text-tertiary);
}

.data-table td {
  padding: var(--space-4);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  border-bottom: 1px solid var(--color-border-light);
}

.data-table tbody tr:last-child td {
  border-bottom: none;
}

.data-table tbody tr:hover {
  background: var(--color-bg-secondary);
}

/* Status badges in tables */
.status-badge {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-1) var(--space-3);
  border-radius: var(--radius-full);
  font-size: var(--text-xs);
  font-weight: var(--font-medium);
}

.status-badge.success {
  background: rgba(16, 185, 129, 0.1);
  color: var(--color-success);
}

.status-badge.pending {
  background: rgba(245, 158, 11, 0.1);
  color: var(--color-warning);
}

.status-badge.error {
  background: rgba(239, 68, 68, 0.1);
  color: var(--color-error);
}
```

---

## Document/File Card Patterns

### Icon + Metadata Layout

```tsx
interface DocCardProps {
  title: string;
  description: string;
  icon: React.ReactNode;
  metadata?: {
    size?: string;
    date?: string;
    author?: string;
  };
  onConnect?: () => void;
}

function DocCard({ title, description, icon, metadata, onConnect }: DocCardProps) {
  return (
    <div className="doc-card">
      <div className="doc-icon">{icon}</div>
      
      <div className="doc-content">
        <h3 className="doc-title">{title}</h3>
        <p className="doc-description">{description}</p>
        
        {metadata && (
          <div className="doc-metadata">
            {metadata.size && <span>{metadata.size}</span>}
            {metadata.date && <span>{metadata.date}</span>}
            {metadata.author && <span>By {metadata.author}</span>}
          </div>
        )}
      </div>
      
      {onConnect && (
        <button className="doc-action" onClick={onConnect}>
          Connect
        </button>
      )}
    </div>
  );
}
```

```css
.doc-card {
  display: flex;
  gap: var(--space-4);
  padding: var(--space-5);
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-xl);
  transition: all 0.2s ease;
}

.doc-card:hover {
  border-color: var(--color-border-medium);
  box-shadow: var(--shadow-md);
}

.doc-icon {
  width: 48px;
  height: 48px;
  border-radius: var(--radius-lg);
  background: var(--color-bg-secondary);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.doc-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}

.doc-title {
  font-size: var(--text-base);
  font-weight: var(--font-semibold);
  color: var(--color-text-primary);
}

.doc-description {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: var(--leading-relaxed);
}

.doc-metadata {
  display: flex;
  gap: var(--space-4);
  font-size: var(--text-xs);
  color: var(--color-text-tertiary);
  margin-top: var(--space-2);
}

.doc-action {
  align-self: flex-start;
  padding: var(--space-2) var(--space-4);
  background: transparent;
  border: 1px solid var(--color-border-medium);
  border-radius: var(--radius-md);
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  color: var(--color-text-secondary);
  cursor: pointer;
  transition: all 0.2s ease;
}

.doc-action:hover {
  background: var(--color-bg-secondary);
  border-color: var(--color-border-strong);
}
```

---

## Folder/Shortcut Grid

```tsx
function ShortcutGrid({ shortcuts }: { shortcuts: Shortcut[] }) {
  return (
    <div className="shortcut-grid">
      {shortcuts.map((shortcut) => (
        <button key={shortcut.id} className="shortcut-item">
          <div className="shortcut-icon">
            <FolderIcon />
          </div>
          <span className="shortcut-label">{shortcut.label}</span>
        </button>
      ))}
    </div>
  );
}
```

```css
.shortcut-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
  gap: var(--space-4);
  margin: var(--space-6) 0;
}

.shortcut-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-5);
  background: transparent;
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-lg);
  cursor: pointer;
  transition: all 0.2s ease;
}

.shortcut-item:hover {
  background: var(--color-bg-secondary);
  border-color: var(--color-border-medium);
  transform: translateY(-2px);
}

.shortcut-icon {
  width: 48px;
  height: 48px;
  color: var(--color-text-tertiary);
}

.shortcut-label {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  text-align: center;
}
```

---

## Activity Feed / Updates List

```tsx
interface Activity {
  id: string;
  type: 'update' | 'new' | 'reassign' | 'feedback';
  title: string;
  description?: string;
  timestamp: string;
  icon?: React.ReactNode;
}

function ActivityFeed({ activities }: { activities: Activity[] }) {
  return (
    <div className="activity-feed">
      <div className="activity-header">
        <h3>Latest Updates</h3>
        <div className="activity-tabs">
          <button className="active">Today</button>
          <button>Yesterday</button>
          <button>This week</button>
        </div>
      </div>
      
      <div className="activity-list">
        {activities.map((activity) => (
          <div key={activity.id} className="activity-item">
            <div className={`activity-icon ${activity.type}`}>
              {activity.icon}
            </div>
            <div className="activity-content">
              <div className="activity-title">{activity.title}</div>
              {activity.description && (
                <div className="activity-description">{activity.description}</div>
              )}
            </div>
            <div className="activity-time">{activity.timestamp}</div>
          </div>
        ))}
      </div>
    </div>
  );
}
```

```css
.activity-feed {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-xl);
  padding: var(--space-6);
}

.activity-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: var(--space-6);
}

.activity-tabs {
  display: flex;
  gap: var(--space-2);
}

.activity-tabs button {
  padding: var(--space-2) var(--space-3);
  background: transparent;
  border: none;
  border-radius: var(--radius-md);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  cursor: pointer;
  transition: all 0.2s ease;
}

.activity-tabs button.active {
  background: var(--color-bg-secondary);
  color: var(--color-text-primary);
  font-weight: var(--font-medium);
}

.activity-list {
  display: flex;
  flex-direction: column;
  gap: var(--space-4);
}

.activity-item {
  display: flex;
  gap: var(--space-3);
  padding: var(--space-3);
  border-radius: var(--radius-md);
  transition: background 0.2s ease;
}

.activity-item:hover {
  background: var(--color-bg-secondary);
}

.activity-icon {
  width: 32px;
  height: 32px;
  border-radius: var(--radius-md);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.activity-icon.update {
  background: rgba(99, 102, 241, 0.1);
  color: var(--color-accent);
}

.activity-icon.new {
  background: rgba(16, 185, 129, 0.1);
  color: var(--color-success);
}

.activity-content {
  flex: 1;
}

.activity-title {
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  color: var(--color-text-primary);
}

.activity-description {
  font-size: var(--text-xs);
  color: var(--color-text-tertiary);
  margin-top: var(--space-1);
}

.activity-time {
  font-size: var(--text-xs);
  color: var(--color-text-tertiary);
  flex-shrink: 0;
}
```

---

## Search & Filter Patterns

```tsx
function SearchWithFilters() {
  return (
    <div className="search-filters">
      <div className="search-box">
        <SearchIcon />
        <input type="text" placeholder="Search..." />
        <kbd>⌘ F</kbd>
      </div>
      
      <div className="filter-buttons">
        <button className="filter-btn">
          <FilterIcon />
          Filter
        </button>
        <button className="filter-btn">
          <GridIcon />
        </button>
        <button className="filter-btn">
          <ListIcon />
        </button>
      </div>
    </div>
  );
}
```

```css
.search-filters {
  display: flex;
  gap: var(--space-3);
  align-items: center;
  margin-bottom: var(--space-6);
}

.search-box {
  flex: 1;
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-lg);
  transition: all 0.2s ease;
}

.search-box:focus-within {
  background: var(--color-bg-primary);
  border-color: var(--color-border-medium);
  box-shadow: var(--shadow-sm);
}

.search-box input {
  flex: 1;
  border: none;
  outline: none;
  background: transparent;
  font-size: var(--text-sm);
  color: var(--color-text-primary);
}

.search-box kbd {
  padding: var(--space-1) var(--space-2);
  background: var(--color-bg-tertiary);
  border-radius: var(--radius-sm);
  font-size: var(--text-xs);
  color: var(--color-text-tertiary);
}

.filter-buttons {
  display: flex;
  gap: var(--space-2);
}

.filter-btn {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-4);
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-lg);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  cursor: pointer;
  transition: all 0.2s ease;
}

.filter-btn:hover {
  background: var(--color-bg-tertiary);
  border-color: var(--color-border-medium);
}
```

---

## Date Range Picker

```css
.date-range-picker {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-4);
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-lg);
  font-size: var(--text-sm);
  cursor: pointer;
}

.date-range-picker:hover {
  border-color: var(--color-border-medium);
}
```

---

## Key Takeaways for Dashboards

### Layout Principles:
1. **Clear hierarchy**: Main stats at top, detailed data below
2. **Grid-based**: Use consistent grid system (12-column)
3. **Generous spacing**: Don't cram components together
4. **Responsive**: Mobile-first, stack on smaller screens

### Visual Patterns:
1. **Soft shadows**: Subtle elevation, not harsh borders
2. **Rounded corners**: 12-16px for cards, 8px for smaller elements
3. **Muted colors**: Gray-based palette with accent colors for data
4. **Typography scale**: Clear hierarchy from headlines to captions

### Interaction Patterns:
1. **Hover states**: Subtle background change + slight elevation
2. **Loading states**: Skeleton screens, not spinners
3. **Empty states**: Helpful messages with actions
4. **Keyboard shortcuts**: Display shortcuts with `<kbd>` tags

---

## Related Guides
- [Modern Design Patterns](./modern-design-patterns.md) - Contemporary UI patterns
- [UI/UX Design](./ui-ux-design.md) - Design systems
- [Frontend Development](./frontend.md) - Implementation
