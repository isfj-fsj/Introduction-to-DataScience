<template>
  <div
    class="h-screen w-full flex flex-col bg-gradient-to-br from-slate-50 via-blue-50 to-indigo-50 overflow-hidden"
  >
    <!-- Page Title Header -->
    <div class="header-gradient text-white py-6 shadow-2xl relative overflow-hidden">
      <div class="absolute inset-0 bg-pattern opacity-10"></div>
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
        <div class="text-center space-y-2">
          <h1 class="text-3xl md:text-4xl font-bold tracking-tight title-gradient">
            Finland Unemployment Rate Regression Analysis
          </h1>
          <div class="flex items-center justify-center gap-2">
            <div class="h-px w-8 bg-blue-200"></div>
            <p class="text-blue-100 text-sm md:text-base font-light tracking-wide">
              Finland Unemployment Rate Regression Analysis
            </p>
            <div class="h-px w-8 bg-blue-200"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Enhanced Controls Section -->
    <div class="bg-white/80 backdrop-blur-sm shadow-xl sticky top-0 z-20">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-5">
        <!-- Time Selectors Row -->
        <div class="flex items-center justify-center gap-3 mb-4">
          <div
            class="flex items-center gap-1.5 bg-gradient-to-r from-blue-50 to-indigo-50 px-2.5 py-1.5 rounded-md shadow-sm"
          >
            <svg
              class="min-w-[14px] text-blue-600"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
              ></path>
            </svg>
            <span class="text-[14px] font-medium text-gray-700">Year</span>
            <a-dropdown :trigger="['click']" placement="bottomLeft">
              <a-button
                type="text"
                class="h-7 min-w-[80px] max-w-[80px] flex items-center justify-between hover:border-blue-400 hover:shadow-md transition-all border-blue-200 text-xs"
              >
                <span class="font-medium text-gray-800">{{ selectedYear }}</span>
                <down-outlined class="ml-1.5 text-xs text-blue-600" />
              </a-button>
              <template #overlay>
                <a-menu
                  @click="handleYearSelect"
                  :selected-keys="[selectedYear.toString()]"
                  class="max-h-[300px] overflow-y-auto rounded-lg shadow-xl"
                >
                  <a-menu-item
                    v-for="year in availableYears"
                    :key="year"
                    class="hover:bg-blue-50 text-xs"
                  >
                    {{ year }}
                  </a-menu-item>
                </a-menu>
              </template>
            </a-dropdown>
          </div>

          <div
            class="flex items-center gap-1.5 bg-gradient-to-r from-purple-50 to-pink-50 px-2.5 py-1.5 rounded-md shadow-sm"
          >
            <svg
              class="min-w-[14px] text-purple-600"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
              ></path>
            </svg>
            <span class="text-[14px] font-medium text-gray-700">Month</span>
            <a-dropdown :trigger="['click']" placement="bottomLeft">
              <a-button
                type="text"
                class="h-7 min-w-[95px] max-w-[95px] flex items-center justify-between hover:border-purple-400 hover:shadow-md transition-all border-purple-200 text-xs"
              >
                <span class="font-medium text-gray-800">{{ selectedMonth }}</span>
                <down-outlined class="ml-1.5 text-xs text-purple-600" />
              </a-button>
              <template #overlay>
                <a-menu
                  @click="handleMonthSelect"
                  :selected-keys="[selectedMonth.toString()]"
                  class="max-h-[300px] overflow-y-auto rounded-lg shadow-xl"
                >
                  <a-menu-item
                    v-for="month in availableMonths"
                    :key="month"
                    class="hover:bg-purple-50 text-xs"
                  >
                    {{ month }}
                  </a-menu-item>
                </a-menu>
              </template>
            </a-dropdown>
          </div>
        </div>

        <!-- View Mode Tabs -->
        <div class="flex items-center justify-center gap-5 flex-wrap">
          <!-- Region Button -->
          <a-button
            :type="activeTab === 'region' ? 'primary' : 'default'"
            @click="handleTabChange('region')"
            >Region
          </a-button>

          <!-- Occupation Button -->
          <a-button
            :type="activeTab === 'occupation' ? 'primary' : 'default'"
            @click="handleTabChange('occupation')"
          >
            Occupation
          </a-button>
        </div>
      </div>
    </div>

    <!-- Main Content with Enhanced Map -->
    <div class="flex-1 w-full overflow-hidden">
      <div class="h-full w-full px-3 py-2">
        <div class="flex gap-3 h-full">
          <!-- Left: Map Container -->
          <div
            :class="[
              'bg-white rounded-xl shadow-2xl p-3 relative transition-all duration-300',
              selectedRegion ? 'w-1/2' : 'w-full',
            ]"
          >
            <!-- Occupation Selector (Fixed in top-right corner of map) -->
            <div
              v-if="activeTab === 'occupation'"
              class="absolute top-4 left-4 z-10 flex items-center gap-2 bg-white/95 backdrop-blur-sm px-4 py-2 rounded-lg shadow-lg"
            >
              <svg
                class="min-w-[16px] max-w-[16px] text-indigo-600"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"
                ></path>
              </svg>
              <span class="px-3 text-[13px] min-w-[60px] font-semibold text-gray-700"
                >Select Occupation:</span
              >
              <a-dropdown :trigger="['click']" placement="bottomLeft">
                <a-button
                  type="text"
                  class="h-8 min-w-[100px] flex items-center justify-between hover:border-indigo-400 hover:shadow-md transition-all border-indigo-300 bg-white text-sm"
                >
                  <span class="font-medium text-gray-800">{{ selectedOccupation }}</span>
                  <down-outlined class="ml-2 text-xs text-indigo-600" />
                </a-button>
                <template #overlay>
                  <a-menu
                    @click="handleOccupationSelect"
                    :selected-keys="[selectedOccupation]"
                    class="max-h-[320px] overflow-y-auto rounded-lg shadow-xl"
                  >
                    <a-menu-item
                      v-for="occupation in availableOccupations"
                      :key="occupation"
                      class="hover:bg-indigo-50 text-sm"
                    >
                      <div class="flex items-center gap-2">
                        <div class="w-1.5 h-1.5 rounded-full bg-indigo-400"></div>
                        {{ occupation }}
                      </div>
                    </a-menu-item>
                  </a-menu>
                </template>
              </a-dropdown>
            </div>

            <div ref="chartContainer" class="w-full h-full rounded-xl"></div>
          </div>

          <!-- Right: Unemployment Rate Chart (appears when region is selected) -->
          <div
            v-if="selectedRegion"
            class="w-1/2 bg-white rounded-xl shadow-2xl p-4 animate-slideIn flex flex-col overflow-hidden"
          >
            <div class="flex items-center justify-between mb-3">
              <h3 class="text-lg font-semibold text-gray-800">
                {{ selectedRegion }} -
                {{ activeTab === 'region' ? 'Unemployment Rate' : 'Demand Rate' }} Trend
              </h3>
              <a-button
                type="text"
                @click="selectedRegion = null"
                class="text-gray-400 hover:text-gray-600 transition-colors"
                >X
              </a-button>
            </div>

            <!-- Info Card using Ant Design -->
            <div class="mb-4">
              <a-row :gutter="[12, 12]">
                <!-- Region Name -->
                <a-col :span="12">
                  <a-statistic title="Region Name" :value="selectedRegion">
                    <template #prefix>
                      <EnvironmentOutlined style="color: #1890ff" />
                    </template>
                  </a-statistic>
                </a-col>

                <!-- Current Time -->
                <a-col :span="12">
                  <a-statistic
                    title="Current Time"
                    :value="`${currentTime.year} ${currentTime.month}`"
                  >
                    <template #prefix>
                      <ClockCircleOutlined style="color: #52c41a" />
                    </template>
                  </a-statistic>
                </a-col>

                <!-- Region View: Unemployed Count & Total Population -->
                <template v-if="activeTab === 'region'">
                  <a-col :span="12">
                    <a-statistic
                      title="Unemployed Count"
                      :value="getUnemployedCount(selectedRegion)"
                      :value-style="{ color: '#cf1322' }"
                    >
                      <template #prefix>
                        <UserDeleteOutlined />
                      </template>
                    </a-statistic>
                  </a-col>
                  <a-col :span="12">
                    <a-statistic
                      title="Total Population"
                      :value="getTotalPopulation(selectedRegion)"
                      :value-style="{ color: '#1890ff' }"
                    >
                      <template #prefix>
                        <TeamOutlined />
                      </template>
                    </a-statistic>
                  </a-col>
                </template>

                <!-- Occupation View: Demand Count & Job Vacancies -->
                <template v-else>
                  <a-col :span="12">
                    <a-statistic
                      title="Demand Count"
                      :value="getDemandCount(selectedRegion)"
                      :value-style="{ color: '#52c41a' }"
                    >
                      <template #prefix>
                        <UserAddOutlined />
                      </template>
                    </a-statistic>
                  </a-col>
                  <a-col :span="12">
                    <a-statistic
                      title="Job Vacancies"
                      :value="getVacancyCount(selectedRegion)"
                      :value-style="{ color: '#722ed1' }"
                    >
                      <template #prefix>
                        <DashboardOutlined />
                      </template>
                    </a-statistic>
                  </a-col>
                </template>
              </a-row>
            </div>

            <div ref="lineChartContainer" class="w-full flex-1 rounded-xl min-h-0"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { ref, onMounted, onUnmounted, watch, nextTick } from 'vue'
import * as echarts from 'echarts'
import {
  DownOutlined,
  EnvironmentOutlined,
  ClockCircleOutlined,
  UserDeleteOutlined,
  TeamOutlined,
  UserAddOutlined,
  DashboardOutlined,
} from '@ant-design/icons-vue'

// Reactive states
const activeTab = ref<'region' | 'occupation'>('region')
const selectedYear = ref<number>(2023)
const selectedMonth = ref<string>('January')
const selectedOccupation = ref<string>('IT & Technology')
const selectedRegion = ref<string | null>(null)
const chartContainer = ref<HTMLDivElement | null>(null)
const lineChartContainer = ref<HTMLDivElement | null>(null)
let mapChart: echarts.ECharts | null = null
let lineChart: echarts.ECharts | null = null

// 时间范围配置（可以自定义修改）
const timeRange = ref({
  startYear: 2022,
  startMonth: 'March',
  endYear: 2026,
  endMonth: 'June',
})

// 当前时间配置（可以自定义修改）
const currentTime = ref<{ year: number; month: string }>({
  year: 2023,
  month: 'June',
})

// Available options
const availableYears = [2020, 2021, 2022, 2023, 2024]
const availableMonths = [
  'January',
  'February',
  'March',
  'April',
  'May',
  'June',
  'July',
  'August',
  'September',
  'October',
  'November',
  'December',
]
const availableOccupations = [
  'IT & Technology',
  'Healthcare',
  'Education & Training',
  'Manufacturing',
  'Service Industry',
  'Construction & Engineering',
  'Finance',
  'Sales & Retail',
]

// 芬兰所有地区列表
const regions = [
  'Lappi',
  'Pohjois-Pohjanmaa',
  'Kainuu',
  'Pohjois-Karjala',
  'Pohjois-Savo',
  'Etelä-Savo',
  'Etelä-Karjala',
  'Kymenlaakso',
  'Keski-Suomi',
  'Etelä-Pohjanmaa',
  'Pohjanmaa',
  'Keski-Pohjanmaa',
  'Pirkanmaa',
  'Satakunta',
  'Varsinais-Suomi',
  'Kanta-Häme',
  'Päijät-Häme',
  'Uusimaa',
  'Ahvenanmaa',
]

// 生成动态数据的函数 - 根据年份、月份和职业生成数据
const generateRegionData = (
  year: number,
  month: string,
  isRegionView: boolean,
  occupation?: string,
) => {
  // 基础人口数据
  const basePopulation: Record<string, number> = {
    Lappi: 180000,
    'Pohjois-Pohjanmaa': 410000,
    Kainuu: 75000,
    'Pohjois-Karjala': 160000,
    'Pohjois-Savo': 245000,
    'Etelä-Savo': 150000,
    'Etelä-Karjala': 130000,
    Kymenlaakso: 170000,
    'Keski-Suomi': 275000,
    'Etelä-Pohjanmaa': 190000,
    Pohjanmaa: 180000,
    'Keski-Pohjanmaa': 68000,
    Pirkanmaa: 520000,
    Satakunta: 220000,
    'Varsinais-Suomi': 480000,
    'Kanta-Häme': 172000,
    'Päijät-Häme': 200000,
    Uusimaa: 1700000,
    Ahvenanmaa: 30000,
  }

  // 不同职业的就业率（相对于人口的比例）
  const occupationRates: Record<string, number> = {
    'IT & Technology': 0.08,
    Healthcare: 0.12,
    'Education & Training': 0.1,
    Manufacturing: 0.15,
    'Service Industry': 0.18,
    'Construction & Engineering': 0.09,
    Finance: 0.06,
    'Sales & Retail': 0.14,
  }

  // 月份索引（用于计算季节性变化）
  const monthIndex = availableMonths.indexOf(month)

  // 年份因子（模拟年度增长/下降）
  const yearFactor = 1 + (year - 2020) * 0.02 // 每年增长2%

  // 月份因子（模拟季节性波动）
  const monthFactor = 1 + Math.sin((monthIndex * Math.PI) / 6) * 0.05 // ±5%的季节性波动

  return regions.map((region) => {
    let value: number

    if (isRegionView) {
      // 地区视图：显示总人口，受年份影响
      const population = basePopulation[region]
      if (population === undefined) {
        value = 0
      } else {
        value = Math.round(population * yearFactor)
      }
    } else {
      // 职业视图：显示该职业的从业人数
      const rate = occupationRates[occupation || 'IT & Technology']
      const population = basePopulation[region]
      if (rate === undefined || population === undefined) {
        value = 0
      } else {
        value = Math.round(population * rate * yearFactor * monthFactor)
      }
    }

    return {
      name: region,
      value: value,
    }
  })
}

// 计算最大值的函数
const getMaxValue = (data: Array<{ name: string; value: number }>) => {
  return Math.max(...data.map((item) => item.value))
}

// 获取总人口（Region View）
const getTotalPopulation = (region: string) => {
  const basePopulation: Record<string, number> = {
    Lappi: 180000,
    'Pohjois-Pohjanmaa': 410000,
    Kainuu: 75000,
    'Pohjois-Karjala': 160000,
    'Pohjois-Savo': 245000,
    'Etelä-Savo': 150000,
    'Etelä-Karjala': 130000,
    Kymenlaakso: 170000,
    'Keski-Suomi': 275000,
    'Etelä-Pohjanmaa': 190000,
    Pohjanmaa: 180000,
    'Keski-Pohjanmaa': 68000,
    Pirkanmaa: 520000,
    Satakunta: 220000,
    'Varsinais-Suomi': 480000,
    'Kanta-Häme': 172000,
    'Päijät-Häme': 200000,
    Uusimaa: 1700000,
    Ahvenanmaa: 30000,
  }

  const yearFactor = 1 + (selectedYear.value - 2020) * 0.02
  return Math.round((basePopulation[region] || 0) * yearFactor)
}

// 获取失业人数（Region View）
const getUnemployedCount = (region: string) => {
  const totalPopulation = getTotalPopulation(region)

  // 获取当前时间的失业率
  const trendData = generateTrendData(region, true)
  const currentIndex = trendData.findIndex(
    (item) => item.year === currentTime.value.year && item.month === currentTime.value.month,
  )

  if (currentIndex === -1) return 0

  const dataPoint = trendData[currentIndex]
  if (!dataPoint) return 0

  const unemploymentRate = dataPoint.rate / 100
  return Math.round(totalPopulation * unemploymentRate)
}

// 获取需求人数（Occupation View）
const getDemandCount = (region: string) => {
  const basePopulation: Record<string, number> = {
    Lappi: 180000,
    'Pohjois-Pohjanmaa': 410000,
    Kainuu: 75000,
    'Pohjois-Karjala': 160000,
    'Pohjois-Savo': 245000,
    'Etelä-Savo': 150000,
    'Etelä-Karjala': 130000,
    Kymenlaakso: 170000,
    'Keski-Suomi': 275000,
    'Etelä-Pohjanmaa': 190000,
    Pohjanmaa: 180000,
    'Keski-Pohjanmaa': 68000,
    Pirkanmaa: 520000,
    Satakunta: 220000,
    'Varsinais-Suomi': 480000,
    'Kanta-Häme': 172000,
    'Päijät-Häme': 200000,
    Uusimaa: 1700000,
    Ahvenanmaa: 30000,
  }

  const occupationRates: Record<string, number> = {
    'IT & Technology': 0.08,
    Healthcare: 0.12,
    'Education & Training': 0.1,
    Manufacturing: 0.15,
    'Service Industry': 0.18,
    'Construction & Engineering': 0.09,
    Finance: 0.06,
    'Sales & Retail': 0.14,
  }

  const population = basePopulation[region] || 0
  const rate = occupationRates[selectedOccupation.value] || 0.08
  const yearFactor = 1 + (selectedYear.value - 2020) * 0.02

  return Math.round(population * rate * yearFactor)
}

// 获取岗位空缺数（Occupation View）
const getVacancyCount = (region: string) => {
  const demandCount = getDemandCount(region)

  // 获取当前时间的需求率（岗位空缺率）
  const trendData = generateTrendData(region, false)
  const currentIndex = trendData.findIndex(
    (item) => item.year === currentTime.value.year && item.month === currentTime.value.month,
  )

  if (currentIndex === -1) return 0

  const dataPoint = trendData[currentIndex]
  if (!dataPoint) return 0

  const demandRate = dataPoint.rate / 100
  // 岗位空缺数 = 需求人数 * (1 - 需求率)，这里假设需求率表示岗位填充率
  // 所以空缺率 = 1 - 需求率/100，然后乘以总需求
  const vacancyRate = 1 - demandRate
  return Math.round(demandCount * vacancyRate)
}

// 生成趋势数据（根据自定义时间范围和视图类型）
const generateTrendData = (region: string, isRegionView: boolean) => {
  const startYear = timeRange.value.startYear
  const endYear = timeRange.value.endYear
  const startMonthIndex = availableMonths.indexOf(timeRange.value.startMonth)
  const endMonthIndex = availableMonths.indexOf(timeRange.value.endMonth)

  const data: Array<{ year: number; month: string; label: string; rate: number }> = []

  if (isRegionView) {
    // Region 视图：显示失业率
    const baseUnemploymentRate: Record<string, number> = {
      Lappi: 8.5,
      'Pohjois-Pohjanmaa': 7.2,
      Kainuu: 9.8,
      'Pohjois-Karjala': 9.1,
      'Pohjois-Savo': 8.3,
      'Etelä-Savo': 8.9,
      'Etelä-Karjala': 8.7,
      Kymenlaakso: 8.4,
      'Keski-Suomi': 7.8,
      'Etelä-Pohjanmaa': 6.9,
      Pohjanmaa: 6.5,
      'Keski-Pohjanmaa': 7.3,
      Pirkanmaa: 6.8,
      Satakunta: 7.6,
      'Varsinais-Suomi': 6.4,
      'Kanta-Häme': 7.5,
      'Päijät-Häme': 7.9,
      Uusimaa: 5.8,
      Ahvenanmaa: 4.2,
    }

    const baseRate = baseUnemploymentRate[region] || 7.5

    for (let year = startYear; year <= endYear; year++) {
      availableMonths.forEach((month, monthIndex) => {
        if (year === startYear && monthIndex < startMonthIndex) return
        if (year === endYear && monthIndex > endMonthIndex) return

        const yearTrend = ((year - startYear) / (endYear - startYear)) * 0.5 - 0.25
        const seasonalFactor = Math.sin((monthIndex * Math.PI) / 6) * 1.2
        const randomFactor = (Math.random() - 0.5) * 0.5

        const rate = baseRate + yearTrend + seasonalFactor + randomFactor

        data.push({
          year,
          month,
          label: `${year}-${month.substring(0, 3)}`,
          rate: Math.max(0, parseFloat(rate.toFixed(2))),
        })
      })
    }
  } else {
    // Occupation 视图：显示需求率
    const baseDemandRate: Record<string, number> = {
      Lappi: 65.0,
      'Pohjois-Pohjanmaa': 72.5,
      Kainuu: 58.0,
      'Pohjois-Karjala': 62.0,
      'Pohjois-Savo': 68.0,
      'Etelä-Savo': 64.5,
      'Etelä-Karjala': 66.0,
      Kymenlaakso: 70.0,
      'Keski-Suomi': 73.5,
      'Etelä-Pohjanmaa': 75.0,
      Pohjanmaa: 77.0,
      'Keski-Pohjanmaa': 71.0,
      Pirkanmaa: 78.5,
      Satakunta: 72.0,
      'Varsinais-Suomi': 80.0,
      'Kanta-Häme': 74.0,
      'Päijät-Häme': 73.0,
      Uusimaa: 85.0,
      Ahvenanmaa: 82.0,
    }

    const baseRate = baseDemandRate[region] || 72.0

    for (let year = startYear; year <= endYear; year++) {
      availableMonths.forEach((month, monthIndex) => {
        if (year === startYear && monthIndex < startMonthIndex) return
        if (year === endYear && monthIndex > endMonthIndex) return

        const yearTrend = ((year - startYear) / (endYear - startYear)) * 3 - 1.5
        const seasonalFactor = Math.sin((monthIndex * Math.PI) / 6) * 2.5
        const randomFactor = (Math.random() - 0.5) * 1.5

        const rate = baseRate + yearTrend + seasonalFactor + randomFactor

        data.push({
          year,
          month,
          label: `${year}-${month.substring(0, 3)}`,
          rate: Math.max(0, Math.min(100, parseFloat(rate.toFixed(2)))), // 需求率在0-100之间
        })
      })
    }
  }

  return data
}

// Handle year selection from dropdown
const handleYearSelect = ({ key }: { key: string }) => {
  selectedYear.value = parseInt(key, 10)
}

// Handle month selection from dropdown
const handleMonthSelect = ({ key }: { key: string }) => {
  selectedMonth.value = key
}

// Handle tab change
const handleTabChange = (tab: 'region' | 'occupation') => {
  activeTab.value = tab
  // 切换 tab 时关闭趋势图
  selectedRegion.value = null
}

// Handle occupation selection
const handleOccupationSelect = ({ key }: { key: string }) => {
  selectedOccupation.value = key
}

// Initialize ECharts map
const initChart = async () => {
  if (!chartContainer.value) return

  // Dispose existing chart instance if any
  if (mapChart) {
    mapChart.dispose()
  }

  // Initialize new chart instance
  mapChart = echarts.init(chartContainer.value)

  // Load Finland GeoJSON map (including all 19 regions with Åland)
  const mapSources = [
    'https://geo.stat.fi/geoserver/wfs?service=WFS&version=2.0.0&request=GetFeature&typeName=tilastointialueet:maakunta4500k&outputFormat=json',
    'https://raw.githubusercontent.com/georgique/world-geojson/master/countries/FIN/regions.geo.json',
    '/maps/finland-regions.json',
  ]

  let loaded = false
  for (const source of mapSources) {
    try {
      console.log(`Trying to load map from: ${source}`)
      const response = await fetch(source)

      if (!response.ok) continue

      const finlandGeoJson = await response.json()

      // Log available regions for verification
      const regionNames = finlandGeoJson.features?.map(
        (f: any) =>
          f.properties?.name || f.properties?.NAME || f.properties?.nimi || f.properties?.maakunta,
      )
      console.log('✅ Loaded regions:', regionNames)

      echarts.registerMap('Finland', finlandGeoJson)
      updateChart()
      loaded = true
      break
    } catch (error) {
      console.warn(`Failed to load from ${source}:`, error)
      continue
    }
  }

  if (!loaded) {
    // Show error message in the chart
    mapChart.setOption({
      title: {
        text: 'Map Loading Failed',
        subtext: 'Unable to load Finland map from any data source',
        left: 'center',
        top: 'center',
        textStyle: {
          color: '#999',
          fontSize: 20,
        },
      },
    })
  }
}

// Handle window resize
const handleResize = () => {
  if (mapChart) {
    mapChart.resize()
  }
  if (lineChart) {
    lineChart.resize()
  }
}

// Initialize line chart for unemployment rate
const initLineChart = () => {
  if (!lineChartContainer.value) return

  if (lineChart) {
    lineChart.dispose()
  }

  lineChart = echarts.init(lineChartContainer.value)
  updateLineChart()

  // 等待动画完成后再次调整大小，确保图表正确渲染
  setTimeout(() => {
    if (lineChart) {
      lineChart.resize()
    }
  }, 350) // 略大于CSS动画时间(300ms)
}

// Update line chart with unemployment data
const updateLineChart = () => {
  if (!lineChart || !selectedRegion.value) return

  const isRegionView = activeTab.value === 'region'
  const trendData = generateTrendData(selectedRegion.value, isRegionView)

  // 找到当前时间点的索引
  const currentIndex = trendData.findIndex(
    (item) => item.year === currentTime.value.year && item.month === currentTime.value.month,
  )

  // 分割数据：当前时间之前（实线）和之后（虚线）
  const historicalData = trendData.map((item, index) => {
    return index <= currentIndex ? item.rate : null
  })

  const forecastData = trendData.map((item, index) => {
    return index >= currentIndex ? item.rate : null
  })

  // 根据视图类型设置标签
  const yAxisName = isRegionView ? 'Unemployment Rate (%)' : 'Demand Rate (%)'
  const seriesName = isRegionView ? 'Unemployment Rate' : 'Demand Rate'

  const option: echarts.EChartsOption = {
    tooltip: {
      trigger: 'axis',
      formatter: (params: any) => {
        const data = params[0]
        if (!data || data.dataIndex === undefined) return ''
        const dataItem = trendData[data.dataIndex]
        if (!dataItem) return ''
        const isForecast = data.dataIndex > currentIndex
        return `${dataItem.year} ${dataItem.month}<br/>${seriesName}: ${data.value}%${isForecast ? ' (Forecast)' : ''}`
      },
      backgroundColor: 'rgba(255, 255, 255, 0.95)',
      borderColor: '#e5e7eb',
      borderWidth: 1,
      textStyle: {
        color: '#374151',
      },
    },
    grid: {
      left: '8%',
      right: '4%',
      top: '10%',
      bottom: '25%',
      containLabel: true,
    },
    // 添加可拖动的时间轴
    dataZoom: [
      {
        type: 'slider',
        xAxisIndex: 0,
        start: 0,
        end: 100,
        height: 15,
        bottom: 12,
        borderColor: 'transparent',
        backgroundColor: '#f5f5f5',
        fillerColor: 'rgba(59, 130, 246, 0.15)',
        handleSize: '120%',
        handleStyle: {
          color: '#3b82f6',
          borderColor: '#fff',
          borderWidth: 2,
          shadowBlur: 4,
          shadowColor: 'rgba(0,0,0,0.2)',
        },
        moveHandleStyle: {
          color: '#3b82f6',
          opacity: 0.5,
        },
        textStyle: {
          color: '#6b7280',
          fontSize: 8,
        },
        showDataShadow: false,
        showDetail: false,
      },
    ],
    xAxis: {
      type: 'category',
      data: trendData.map((item) => item.label),
      axisLabel: {
        rotate: 45,
        fontSize: 10,
        color: '#6b7280',
        interval: 2, // 每隔2个显示一个标签，避免太密集
      },
      axisLine: {
        lineStyle: {
          color: '#d1d5db',
        },
      },
    },
    yAxis: {
      type: 'value',
      name: yAxisName,
      nameTextStyle: {
        color: '#6b7280',
        fontSize: 12,
      },
      axisLabel: {
        formatter: '{value}%',
        fontSize: 11,
        color: '#6b7280',
      },
      axisLine: {
        lineStyle: {
          color: '#d1d5db',
        },
      },
      splitLine: {
        lineStyle: {
          color: '#f3f4f6',
        },
      },
    },
    series: [
      // 历史数据（实线）
      {
        name: `Historical ${seriesName}`,
        type: 'line',
        data: historicalData,
        smooth: true,
        symbol: 'circle',
        symbolSize: 4,
        itemStyle: {
          color: '#3b82f6',
          borderColor: '#fff',
          borderWidth: 1,
        },
        lineStyle: {
          color: '#3b82f6',
          width: 3,
          type: 'solid',
        },
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(59, 130, 246, 0.3)' },
            { offset: 1, color: 'rgba(59, 130, 246, 0.05)' },
          ]),
        },
        emphasis: {
          itemStyle: {
            color: '#1e40af',
            borderColor: '#fff',
            borderWidth: 2,
            shadowBlur: 8,
            shadowColor: 'rgba(59, 130, 246, 0.5)',
          },
          scale: 1.5,
        },
        // 添加当前时间标记线
        markLine: {
          silent: false,
          symbol: 'none',
          label: {
            show: true,
            position: 'end',
            formatter: 'Current Time',
            color: '#ef4444',
            fontSize: 11,
            fontWeight: 'bold',
          },
          lineStyle: {
            color: '#ef4444',
            width: 2,
            type: 'solid',
          },
          data: [
            {
              xAxis: currentIndex,
            },
          ],
        },
      },
      // 预测数据（虚线）
      {
        name: `Forecast ${seriesName}`,
        type: 'line',
        data: forecastData,
        smooth: true,
        symbol: 'circle',
        symbolSize: 4,
        itemStyle: {
          color: '#3b82f6',
          borderColor: '#fff',
          borderWidth: 1,
        },
        lineStyle: {
          color: '#3b82f6',
          width: 3,
          type: 'dashed',
        },
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(59, 130, 246, 0.15)' },
            { offset: 1, color: 'rgba(59, 130, 246, 0.02)' },
          ]),
        },
        emphasis: {
          itemStyle: {
            color: '#1e40af',
            borderColor: '#fff',
            borderWidth: 2,
            shadowBlur: 8,
            shadowColor: 'rgba(59, 130, 246, 0.5)',
          },
          scale: 1.5,
        },
      },
    ],
  }

  lineChart.setOption(option, true)
}
// Update chart based on current tab and year
const updateChart = () => {
  if (!mapChart) return

  const isRegionView = activeTab.value === 'region'

  // 使用动态数据生成函数
  const data = generateRegionData(
    selectedYear.value,
    selectedMonth.value,
    isRegionView,
    selectedOccupation.value,
  )

  // 动态计算最大值
  const maxValue = getMaxValue(data)

  const option: echarts.EChartsOption = {
    tooltip: {
      trigger: 'item',
      formatter: (params: any) => {
        return `${params.name}<br/>${isRegionView ? 'Population' : 'Employees'}: ${params.value ? params.value.toLocaleString() : 'N/A'}`
      },
      backgroundColor: 'rgba(255, 255, 255, 0.95)',
      borderColor: '#e5e7eb',
      borderWidth: 1,
      textStyle: {
        color: '#374151',
      },
    },
    visualMap: {
      min: 0,
      max: maxValue,
      left: 'left',
      bottom: '5%',
      text: ['High', 'Low'],
      textStyle: {
        color: '#6b7280',
      },
      inRange: {
        color: ['#dbeafe', '#3b82f6', '#1e40af'],
      },
      calculable: true,
      orient: 'horizontal',
      padding: 10,
    },
    series: [
      {
        name: isRegionView ? 'Regional Population' : 'Occupation Distribution',
        type: 'map',
        map: 'Finland',
        roam: true,
        scaleLimit: {
          min: 0.8,
          max: 3,
        },
        emphasis: {
          label: {
            show: true,
            color: '#1e40af',
          },
          itemStyle: {
            areaColor: '#60a5fa',
            borderColor: 'transparent',
            borderWidth: 0,
          },
        },
        itemStyle: {
          areaColor: '#e5e7eb',
          borderColor: 'transparent',
          borderWidth: 0,
        },
        label: {
          show: true,
          fontSize: 10,
          color: '#4b5563',
        },
        data: data,
      },
    ],
  }

  mapChart.setOption(option, true)

  // Add click event listener to map
  mapChart.off('click') // Remove previous listeners
  mapChart.on('click', (params: any) => {
    if (params.componentType === 'series' && params.name) {
      console.log('点击的地区:', params.name)
      // 如果点击的是同一个地区，则取消选择
      if (selectedRegion.value === params.name) {
        selectedRegion.value = null
      } else {
        // 否则选择新的地区
        selectedRegion.value = params.name
      }
    }
  })
}
// Watchers for reactive updates
watch(activeTab, () => {
  nextTick(() => {
    updateChart()
  })
})
watch(selectedYear, () => {
  // In a real application, this would filter the data by year
  nextTick(() => {
    updateChart()
  })
})
watch(selectedMonth, () => {
  // In a real application, this would filter the data by month
  nextTick(() => {
    updateChart()
  })
})
watch(selectedOccupation, () => {
  // Update chart when occupation changes
  nextTick(() => {
    updateChart()
  })
})
watch(selectedRegion, (newRegion) => {
  if (newRegion) {
    nextTick(() => {
      initLineChart()
      // Resize map chart after layout change
      setTimeout(() => {
        if (mapChart) {
          mapChart.resize()
        }
      }, 300)
    })
  } else {
    // Dispose line chart when region is deselected
    if (lineChart) {
      lineChart.dispose()
      lineChart = null
    }
    // Resize map chart after layout change
    setTimeout(() => {
      if (mapChart) {
        mapChart.resize()
      }
    }, 300)
  }
})
// Lifecycle hooks
onMounted(() => {
  nextTick(() => {
    initChart()
  })
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  if (mapChart) {
    mapChart.dispose()
  }
  if (lineChart) {
    lineChart.dispose()
  }
})
</script>
<style scoped>
/* Header Gradient with Animation */
.header-gradient {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
  background-size: 200% 200%;
  animation: gradientShift 8s ease infinite;
}

@keyframes gradientShift {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

/* Background Pattern */
.bg-pattern {
  background-image:
    radial-gradient(circle at 20% 50%, rgba(255, 255, 255, 0.2) 0%, transparent 50%),
    radial-gradient(circle at 80% 80%, rgba(255, 255, 255, 0.15) 0%, transparent 50%),
    radial-gradient(circle at 40% 20%, rgba(255, 255, 255, 0.1) 0%, transparent 50%);
}

/* Title Gradient Effect */
.title-gradient {
  background: linear-gradient(to right, #ffffff 0%, #e0e7ff 50%, #ffffff 100%);
  background-size: 200% auto;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 3s linear infinite;
}

@keyframes shimmer {
  0% {
    background-position: 0% center;
  }
  100% {
    background-position: 200% center;
  }
}

/* Slide In Animation */
@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.animate-slideIn {
  animation: slideIn 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Pulse Animation */
@keyframes pulse {
  0%,
  100% {
    opacity: 0.2;
  }
  50% {
    opacity: 0.3;
  }
}

.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

/* Smooth transitions */
button,
.transition-all {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Enhanced shadows */
.shadow-glow {
  box-shadow: 0 0 20px rgba(99, 102, 241, 0.4);
}

/* Hover effects for buttons */
button:active {
  transform: scale(0.98);
}

/* Custom scrollbar for dropdowns */
:deep(.ant-menu) {
  scrollbar-width: thin;
  scrollbar-color: rgba(99, 102, 241, 0.3) transparent;
}

:deep(.ant-menu::-webkit-scrollbar) {
  width: 6px;
}

:deep(.ant-menu::-webkit-scrollbar-track) {
  background: transparent;
}

:deep(.ant-menu::-webkit-scrollbar-thumb) {
  background-color: rgba(99, 102, 241, 0.3);
  border-radius: 3px;
}

:deep(.ant-menu::-webkit-scrollbar-thumb:hover) {
  background-color: rgba(99, 102, 241, 0.5);
}

/* Enhanced menu items */
:deep(.ant-menu-item) {
  transition: all 0.2s ease;
  border-radius: 4px;
  margin: 2px 4px;
}

:deep(.ant-menu-item-selected) {
  background: linear-gradient(
    135deg,
    rgba(99, 102, 241, 0.1) 0%,
    rgba(139, 92, 246, 0.1) 100%
  ) !important;
  font-weight: 600;
  color: #6366f1;
}

/* Icon animations */
svg {
  transition: transform 0.3s ease;
}

button:hover svg {
  transform: scale(1.1);
}

/* Backdrop blur effect */
.backdrop-blur-sm {
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
}

/* Glass morphism effect */
.glass-effect {
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}
</style>
