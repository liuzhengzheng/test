<script setup>
import { ref } from 'vue'
import { mockUsers } from '../data/mockUsers.js'

const navItems = [
  { id: 'dashboard', label: '仪表盘111' },
  { id: 'users', label: '用户管理' },
  { id: 'settings', label: '设置' },
]

const activeNav = ref('users')
const users = ref(mockUsers)

const statusLabel = (s) => (s === 'active' ? '正常' : '禁用')
</script>

<template>
  <div class="admin-layout">
    <aside class="sidebar" aria-label="管理后台导航">
      <div class="sidebar__brand">管理后台</div>
      <nav class="sidebar__nav">
        <button
          v-for="item in navItems"
          :key="item.id"
          type="button"
          class="nav-item"
          :class="{ 'nav-item--active': activeNav === item.id }"
          @click="activeNav = item.id"
        >
          {{ item.label }}
        </button>
      </nav>
    </aside>

    <main class="main">
      <header class="main__header">
        <h1 class="main__title">用户列表</h1>
        <p class="main__sub">共 {{ users.length }} 位用户（Mock 数据）</p>
      </header>

      <div class="table-wrap">
        <table class="user-table">
          <thead>
            <tr>
              <th scope="col">ID</th>
              <th scope="col">姓名</th>
              <th scope="col">邮箱</th>
              <th scope="col">角色</th>
              <th scope="col">状态</th>
              <th scope="col">创建时间</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="u in users" :key="u.id">
              <td>{{ u.id }}</td>
              <td>{{ u.name }}</td>
              <td>{{ u.email }}</td>
              <td>{{ u.role }}</td>
              <td>
                <span class="status" :class="`status--${u.status}`">
                  {{ statusLabel(u.status) }}
                </span>
              </td>
              <td>{{ u.createdAt }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </main>
  </div>
</template>

<style scoped>
.admin-layout {
  display: flex;
  min-height: 100svh;
  width: 100%;
  background: var(--bg);
  color: var(--text);
}

.sidebar {
  width: 220px;
  flex-shrink: 0;
  border-right: 1px solid var(--border);
  background: var(--code-bg);
  display: flex;
  flex-direction: column;
  padding: 20px 0;
}

.sidebar__brand {
  padding: 0 20px 20px;
  font-weight: 600;
  font-size: 1.05rem;
  color: var(--text-h);
  border-bottom: 1px solid var(--border);
  margin-bottom: 12px;
}

.sidebar__nav {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 0 10px;
}

.nav-item {
  text-align: left;
  font: inherit;
  font-size: 0.95rem;
  padding: 10px 12px;
  border: none;
  border-radius: 8px;
  background: transparent;
  color: var(--text);
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}

.nav-item:hover {
  background: var(--social-bg, rgba(0, 0, 0, 0.05));
  color: var(--text-h);
}

.nav-item--active {
  background: var(--accent-bg);
  color: var(--accent);
  font-weight: 500;
}

.nav-item--active:hover {
  color: var(--accent);
}

.main {
  flex: 1;
  min-width: 0;
  padding: 28px 32px 40px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.main__header {
  margin: 0;
}

.main__title {
  font-size: 1.5rem;
  font-weight: 600;
  margin: 0 0 6px;
  color: var(--text-h);
}

.main__sub {
  margin: 0;
  font-size: 0.9rem;
  opacity: 0.9;
}

.table-wrap {
  overflow: auto;
  border: 1px solid var(--border);
  border-radius: 10px;
  background: var(--bg);
  box-shadow: var(--shadow);
}

.user-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.92rem;
}

.user-table th,
.user-table td {
  padding: 12px 16px;
  text-align: left;
  border-bottom: 1px solid var(--border);
}

.user-table th {
  font-weight: 600;
  color: var(--text-h);
  background: var(--code-bg);
  white-space: nowrap;
}

.user-table tbody tr:last-child td {
  border-bottom: none;
}

.user-table tbody tr:hover {
  background: var(--social-bg);
}

.status {
  display: inline-block;
  padding: 2px 10px;
  border-radius: 999px;
  font-size: 0.8rem;
  font-weight: 500;
}

.status--active {
  background: var(--accent-bg);
  color: var(--accent);
}

.status--inactive {
  background: rgba(148, 163, 184, 0.25);
  color: var(--text-h);
}

@media (max-width: 768px) {
  .admin-layout {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    border-right: none;
    border-bottom: 1px solid var(--border);
    padding: 12px 0;
  }

  .sidebar__brand {
    margin-bottom: 8px;
    padding-bottom: 12px;
  }

  .sidebar__nav {
    flex-direction: row;
    flex-wrap: wrap;
  }

  .main {
    padding: 20px 16px 32px;
  }
}
</style>
