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
            Work in Finland
          </h1>
          <div class="flex items-center justify-center gap-2">
            <div class="h-px w-8 bg-blue-200"></div>
            <p class="text-blue-100 text-sm md:text-base font-light tracking-wide">
              Work in Finland
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
        <div class="flex items-center justify-center gap-[20px] flex-wrap">
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

                <!-- Selected Time -->
                <a-col :span="12">
                  <a-statistic title="Selected Time" :value="`${selectedYear} ${selectedMonth}`">
                    <template #prefix>
                      <ClockCircleOutlined style="color: #52c41a" />
                    </template>
                  </a-statistic>
                </a-col>

                <!-- Region View: Unemployed Count & Total Workforce -->
                <template v-if="activeTab === 'region'">
                  <a-col :span="12">
                    <a-statistic
                      title="Unemployed Count"
                      :value="getUnemployedCountForSelectedTime(selectedRegion)"
                      :value-style="{ color: '#cf1322' }"
                    >
                      <template #prefix>
                        <UserDeleteOutlined />
                      </template>
                    </a-statistic>
                  </a-col>
                  <a-col :span="12">
                    <a-statistic
                      title="Total Workforce"
                      :value="getTotalPopulationForSelectedTime(selectedRegion)"
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
import { ref, onMounted, onUnmounted, watch, nextTick, computed } from 'vue'
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
import region_data from '../assets/data1_with_region.json'
// import occupation_data from '../assets/data2_with_Job.json'
import occupation_data from '../assets/data2_with_Job.json'

const regionData = ref<any>(region_data)
const occupationData = ref<any>(occupation_data)
console.log(regionData.value.data)
console.log(occupationData.value.data)

// 从JSON数据中提取可用的年份和月份（Region视图）
const extractAvailableYearsAndMonthsForRegion = () => {
  const yearsSet = new Set<number>()
  const monthsByYear: Record<number, Set<number>> = {}

  regionData.value.data.forEach((item: any) => {
    const date = new Date(item.time)
    const year = date.getFullYear()
    const month = date.getMonth() + 1 // 0-11 -> 1-12

    yearsSet.add(year)
    if (!monthsByYear[year]) {
      monthsByYear[year] = new Set()
    }
    monthsByYear[year].add(month)
  })

  return {
    years: Array.from(yearsSet).sort((a, b) => a - b),
    monthsByYear,
  }
}

// 从JSON数据中提取可用的年份和月份（Occupation视图）
const extractAvailableYearsAndMonthsForOccupation = () => {
  const yearsSet = new Set<number>()
  const monthsByYear: Record<number, Set<number>> = {}

  occupationData.value.data.forEach((item: any) => {
    const date = new Date(item.time)
    const year = date.getFullYear()
    const month = date.getMonth() + 1 // 0-11 -> 1-12

    yearsSet.add(year)
    if (!monthsByYear[year]) {
      monthsByYear[year] = new Set()
    }
    monthsByYear[year].add(month)
  })

  return {
    years: Array.from(yearsSet).sort((a, b) => a - b),
    monthsByYear,
  }
}

const regionYearsData = extractAvailableYearsAndMonthsForRegion()
const occupationYearsData = extractAvailableYearsAndMonthsForOccupation()

// 从 occupation JSON 数据中提取所有可用的职业名称
const extractAvailableOccupations = () => {
  const occupationsSet = new Set<string>()
  occupationData.value.data.forEach((item: any) => {
    if (item.job_name) {
      occupationsSet.add(item.job_name)
    }
  })
  return Array.from(occupationsSet).sort()
}

const availableOccupations = extractAvailableOccupations()

// 根据当前视图返回对应的年份和月份数据
const availableYears = computed(() => {
  return activeTab.value === 'region' ? regionYearsData.years : occupationYearsData.years
})

const monthsByYear = computed(() => {
  return activeTab.value === 'region'
    ? regionYearsData.monthsByYear
    : occupationYearsData.monthsByYear
})

// 月份名称映射
const monthNames = [
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

// Reactive states
const activeTab = ref<'region' | 'occupation'>('region')
const selectedYear = ref<any>(2025)
const selectedMonth = ref<string>('August')
const selectedOccupation = ref<string>(
  availableOccupations.length > 0 ? availableOccupations[0] : 'Administration professionals',
)
const selectedRegion = ref<string | null>(null)
const chartContainer = ref<HTMLDivElement | null>(null)
const lineChartContainer = ref<HTMLDivElement | null>(null)
let mapChart: echarts.ECharts | null = null
let lineChart: echarts.ECharts | null = null

// 计算当前选中年份的可用月份
const availableMonths = computed(() => {
  const months = monthsByYear.value[selectedYear.value]
  if (!months) return []
  return Array.from(months)
    .sort((a, b) => a - b)
    .map((m) => monthNames[m - 1])
})

// 当年份改变时，自动选择该年份的第一个可用月份
watch(selectedYear, (newYear) => {
  const months = monthsByYear.value[newYear]
  if (months && months.size > 0) {
    const firstMonth = Math.min(...Array.from(months))
    selectedMonth.value = monthNames[firstMonth - 1]
  }
})

// 获取当前时间（最后一个flag=0的数据点）- Region视图
const getCurrentTimeForRegion = () => {
  const historicalData = regionData.value.data.filter((item: any) => item.flag === 0)
  if (historicalData.length === 0) return null

  const lastHistorical = historicalData[historicalData.length - 1]
  const date = new Date(lastHistorical.time)
  return {
    year: date.getFullYear(),
    month: monthNames[date.getMonth()],
  }
}

// 获取当前时间（最后一个flag=0的数据点）- Occupation视图
const getCurrentTimeForOccupation = () => {
  const historicalData = occupationData.value.data.filter((item: any) => item.flag === 0)
  if (historicalData.length === 0) return null

  const lastHistorical = historicalData[historicalData.length - 1]
  const date = new Date(lastHistorical.time)
  return {
    year: date.getFullYear(),
    month: monthNames[date.getMonth()],
  }
}

const currentTimeRegion = getCurrentTimeForRegion() || { year: 2025, month: 'August' }
const currentTimeOccupation = getCurrentTimeForOccupation() || { year: 2025, month: 'August' }

// 根据当前视图返回对应的当前时间
const currentTime = computed(() => {
  return activeTab.value === 'region' ? currentTimeRegion : currentTimeOccupation
})

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

// 地图显示名称映射到标准地区名称（处理英文和其他别名）
const mapToStandardRegionName: Record<string, string> = {
  // 英文名称别名
  Lapland: 'Lappi',
  'South Karelia': 'Etelä-Karjala',
  'South Ostrobothnia': 'Etelä-Pohjanmaa',
  'South Savo': 'Etelä-Savo',
  Kainuu: 'Kainuu',
  'Tavastia Proper': 'Kanta-Häme',
  'Central Ostrobothnia': 'Keski-Pohjanmaa',
  'Central Finland': 'Keski-Suomi',
  Kymenlaakso: 'Kymenlaakso',
  'Päijänne Tavastia': 'Päijät-Häme',
  Pirkanmaa: 'Pirkanmaa',
  Ostrobothnia: 'Pohjanmaa',
  'North Karelia': 'Pohjois-Karjala',
  'North Ostrobothnia': 'Pohjois-Pohjanmaa',
  'North Savo': 'Pohjois-Savo',
  Satakunta: 'Satakunta',
  'Helsinki-Uusimaa': 'Uusimaa',
  Uusimaa: 'Uusimaa',
  'Southwest Finland': 'Varsinais-Suomi',
  Åland: 'Ahvenanmaa',
  Ahvenanmaa: 'Ahvenanmaa',

  // 芬兰语原名（直接映射）
  Lappi: 'Lappi',
  'Etelä-Karjala': 'Etelä-Karjala',
  'Etelä-Pohjanmaa': 'Etelä-Pohjanmaa',
  'Etelä-Savo': 'Etelä-Savo',
  'Kanta-Häme': 'Kanta-Häme',
  'Keski-Pohjanmaa': 'Keski-Pohjanmaa',
  'Keski-Suomi': 'Keski-Suomi',
  'Päijät-Häme': 'Päijät-Häme',
  Pohjanmaa: 'Pohjanmaa',
  'Pohjois-Karjala': 'Pohjois-Karjala',
  'Pohjois-Pohjanmaa': 'Pohjois-Pohjanmaa',
  'Pohjois-Savo': 'Pohjois-Savo',
  'Varsinais-Suomi': 'Varsinais-Suomi',
}

// JSON数据中的地区名称映射到标准地区名称
const jsonToStandardRegionName: Record<string, string> = {
  Ahvenanmaa: 'Ahvenanmaa',
  'Etela-Karjala': 'Etelä-Karjala',
  'Etela-Pohjanmaa': 'Etelä-Pohjanmaa',
  'Etela-Savo': 'Etelä-Savo',
  Kainuu: 'Kainuu',
  'Kanta-Hame': 'Kanta-Häme',
  'Keski-Pohjanmaa': 'Keski-Pohjanmaa',
  'Keski-Suomi': 'Keski-Suomi',
  Kymenlaakso: 'Kymenlaakso',
  Lappi: 'Lappi',
  'Paijat-Hame': 'Päijät-Häme',
  Pirkanmaa: 'Pirkanmaa',
  Pohjanmaa: 'Pohjanmaa',
  'Pohjois-Karjala': 'Pohjois-Karjala',
  'Pohjois-Pohjanmaa': 'Pohjois-Pohjanmaa',
  'Pohjois-Savo': 'Pohjois-Savo',
  Satakunta: 'Satakunta',
  Uusimaa: 'Uusimaa',
  'Varsinais-Suomi': 'Varsinais-Suomi',
}

// 标准地区名称映射回JSON中的名称（用于查询）
const standardToJsonRegionName: Record<string, string> = {}
Object.keys(jsonToStandardRegionName).forEach((key) => {
  standardToJsonRegionName[jsonToStandardRegionName[key]] = key
})

// 标准化地区名称（从JSON格式转换为标准格式）
const normalizeRegionName = (name: string): string => {
  return jsonToStandardRegionName[name] || name
}

// 反向标准化（从标准格式转换为JSON格式）
const denormalizeRegionName = (name: string): string => {
  return standardToJsonRegionName[name] || name
}

// 从地图名称转换为标准地区名称（处理地图显示的各种别名）
const mapNameToStandardRegionName = (mapName: string): string => {
  return mapToStandardRegionName[mapName] || mapName
}

// 从标准地区名称转换为地图显示名称（反向查找）
const standardToMapRegionName: Record<string, string[]> = {}
Object.keys(mapToStandardRegionName).forEach((mapName) => {
  const standardName = mapToStandardRegionName[mapName]
  if (!standardToMapRegionName[standardName]) {
    standardToMapRegionName[standardName] = []
  }
  standardToMapRegionName[standardName].push(mapName)
})

// 获取某个标准区域名称的所有可能的地图显示名称
const getMapNamesForStandardRegion = (standardName: string): string[] => {
  return standardToMapRegionName[standardName] || [standardName]
}

// 从JSON数据生成地图展示数据 - 根据年份、月份生成数据
const generateRegionData = (
  year: number,
  month: string,
  isRegionView: boolean,
  occupation?: string,
) => {
  if (!isRegionView) {
    // Occupation视图：从JSON数据读取真实数据
    const monthIndex = monthNames.indexOf(month) + 1 // 1-12
    const result: any[] = []

    regions.forEach((region) => {
      // 将标准地区名称转换为JSON中的格式进行查询
      const jsonRegionName = denormalizeRegionName(region)

      // 查找对应的数据
      const dataItem = occupationData.value.data.find((item: any) => {
        const itemDate = new Date(item.time)
        const itemYear = itemDate.getFullYear()
        const itemMonth = itemDate.getMonth() + 1

        return (
          itemYear === year &&
          itemMonth === monthIndex &&
          item.region_name === jsonRegionName &&
          item.job_name === occupation
        )
      })

      const dataValue = dataItem
        ? {
            value: parseFloat(dataItem.Vacancy_Jobseeker_Ratio.toFixed(2)), // 直接显示数值
            job_hunters: dataItem.job_hunters,
            job_vacancy: dataItem.job_vacancy,
          }
        : {
            value: 0,
            job_hunters: 0,
            job_vacancy: 0,
          }

      // 获取该标准区域名称的所有可能的地图别名
      const mapNames = getMapNamesForStandardRegion(region)
      mapNames.forEach((mapName) => {
        result.push({
          name: mapName,
          ...dataValue,
        })
      })
    })

    return result
  }

  // Region视图：从JSON数据读取真实数据
  const monthIndex = monthNames.indexOf(month) + 1 // 1-12
  const result: any[] = []

  regions.forEach((region) => {
    // 将标准地区名称转换为JSON中的格式进行查询
    const jsonRegionName = denormalizeRegionName(region)

    // 查找对应的数据
    const dataItem = regionData.value.data.find((item: any) => {
      const itemDate = new Date(item.time)
      const itemYear = itemDate.getFullYear()
      const itemMonth = itemDate.getMonth() + 1

      return itemYear === year && itemMonth === monthIndex && item.region_name === jsonRegionName
    })

    const dataValue = dataItem
      ? {
          value: parseFloat(dataItem.Unemployment_Rate.toFixed(2)),
          unemployment_people: dataItem.unemployment_people,
          all_people: dataItem.all_people,
        }
      : {
          value: 0,
          unemployment_people: 0,
          all_people: 0,
        }

    // 获取该标准区域名称的所有可能的地图别名
    const mapNames = getMapNamesForStandardRegion(region)
    mapNames.forEach((mapName) => {
      result.push({
        name: mapName,
        ...dataValue,
      })
    })
  })

  return result
}

// 计算最大值的函数
const getMaxValue = (data: Array<{ name: string; value: number }>) => {
  return Math.max(...data.map((item) => item.value))
}

// 获取当前时间的数据（用于标记当前时间线）
const getCurrentTimeData = (region: string) => {
  const monthIndex = monthNames.indexOf(currentTime.value.month) + 1
  const jsonRegionName = denormalizeRegionName(region)

  const dataItem = regionData.value.data.find((item: any) => {
    const itemDate = new Date(item.time)
    const itemYear = itemDate.getFullYear()
    const itemMonth = itemDate.getMonth() + 1

    return (
      itemYear === currentTime.value.year &&
      itemMonth === monthIndex &&
      item.region_name === jsonRegionName
    )
  })

  return dataItem || null
}

// 获取用户选择时间点的数据（Region View）
const getSelectedTimeData = (region: string) => {
  const monthIndex = monthNames.indexOf(selectedMonth.value) + 1
  const jsonRegionName = denormalizeRegionName(region)

  const dataItem = regionData.value.data.find((item: any) => {
    const itemDate = new Date(item.time)
    const itemYear = itemDate.getFullYear()
    const itemMonth = itemDate.getMonth() + 1

    return (
      itemYear === selectedYear.value &&
      itemMonth === monthIndex &&
      item.region_name === jsonRegionName
    )
  })

  return dataItem || null
}

// 获取用户选择时间点的总人口（Region View）
const getTotalPopulationForSelectedTime = (region: string) => {
  const data = getSelectedTimeData(region)
  return data ? Math.round(data.all_people) : 0
}

// 获取用户选择时间点的失业人数（Region View）
const getUnemployedCountForSelectedTime = (region: string) => {
  const data = getSelectedTimeData(region)
  return data ? Math.round(data.unemployment_people) : 0
}

// 获取用户选择时间点的数据（Occupation View）
const getSelectedTimeDataForOccupation = (region: string) => {
  const monthIndex = monthNames.indexOf(selectedMonth.value) + 1
  const jsonRegionName = denormalizeRegionName(region)

  const dataItem = occupationData.value.data.find((item: any) => {
    const itemDate = new Date(item.time)
    const itemYear = itemDate.getFullYear()
    const itemMonth = itemDate.getMonth() + 1

    return (
      itemYear === selectedYear.value &&
      itemMonth === monthIndex &&
      item.region_name === jsonRegionName &&
      item.job_name === selectedOccupation.value
    )
  })

  return dataItem || null
}

// 获取找工作的人数（Occupation View）
const getDemandCount = (region: string) => {
  const data = getSelectedTimeDataForOccupation(region)
  return data ? Math.round(data.job_hunters) : 0
}

// 获取岗位空缺数（Occupation View）
const getVacancyCount = (region: string) => {
  const data = getSelectedTimeDataForOccupation(region)
  return data ? Math.round(data.job_vacancy) : 0
}

// 生成趋势数据（从JSON读取该区域的所有数据）
const generateTrendData = (region: string, isRegionView: boolean) => {
  if (!isRegionView) {
    // Occupation 视图：从JSON数据读取真实数据
    const jsonRegionName = denormalizeRegionName(region)
    const occupationDataItems = occupationData.value.data.filter((item: any) => {
      return item.region_name === jsonRegionName && item.job_name === selectedOccupation.value
    })

    // 按时间排序
    occupationDataItems.sort((a: any, b: any) => {
      return new Date(a.time).getTime() - new Date(b.time).getTime()
    })

    // 转换为图表数据格式
    return occupationDataItems.map((item: any) => {
      const date = new Date(item.time)
      const year = date.getFullYear()
      const month = monthNames[date.getMonth()]

      return {
        year,
        month,
        label: `${year}-${month.substring(0, 3)}`,
        rate: parseFloat(item.Vacancy_Jobseeker_Ratio.toFixed(2)), // 直接显示数值
        flag: item.flag,
        job_hunters: item.job_hunters,
        job_vacancy: item.job_vacancy,
      }
    })
  }

  // Region 视图：从JSON数据读取真实数据
  const jsonRegionName = denormalizeRegionName(region)
  const regionDataItems = regionData.value.data.filter((item: any) => {
    return item.region_name === jsonRegionName
  })

  // 按时间排序
  regionDataItems.sort((a: any, b: any) => {
    return new Date(a.time).getTime() - new Date(b.time).getTime()
  })

  // 转换为图表数据格式
  return regionDataItems.map((item: any) => {
    const date = new Date(item.time)
    const year = date.getFullYear()
    const month = monthNames[date.getMonth()]

    return {
      year,
      month,
      label: `${year}-${month.substring(0, 3)}`,
      rate: parseFloat(item.Unemployment_Rate.toFixed(2)),
      flag: item.flag,
      unemployment_people: item.unemployment_people,
      all_people: item.all_people,
    }
  })
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

  // 切换视图时，重置年份和月份为新视图的第一个可用值
  const years = tab === 'region' ? regionYearsData.years : occupationYearsData.years
  const monthsData =
    tab === 'region' ? regionYearsData.monthsByYear : occupationYearsData.monthsByYear

  if (years.length > 0) {
    selectedYear.value = years[0]
    const months = monthsData[years[0]]
    if (months && months.size > 0) {
      const firstMonth = Math.min(...Array.from(months))
      selectedMonth.value = monthNames[firstMonth - 1]
    }
  }
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

  // 找到最后一个flag=0的数据点索引（当前时间）
  let currentIndex = -1
  for (let i = trendData.length - 1; i >= 0; i--) {
    // flag可能是数字0或字符串，统一比较
    if (trendData[i].flag === 0 || String(trendData[i].flag) === '0') {
      currentIndex = i
      break
    }
  }

  // 如果没有找到flag=0的数据，使用时间匹配
  if (currentIndex === -1) {
    currentIndex = trendData.findIndex(
      (item) => item.year === currentTime.value.year && item.month === currentTime.value.month,
    )
  }

  // 分割数据：flag=0（实线）和flag=1/1J（虚线）
  const historicalData = trendData.map((item, index) => {
    // flag为0时显示历史数据
    const isHistorical = item.flag === 0 || String(item.flag) === '0'
    return isHistorical ? item.rate : null
  })

  const forecastData = trendData.map((item, index) => {
    // flag为'1'或'1J'时显示预测数据
    const isForecast = item.flag !== 0 && String(item.flag) !== '0'
    return isForecast ? item.rate : null
  })

  // 为了连接实线和虚线，在交界处添加数据点
  if (currentIndex >= 0 && currentIndex < trendData.length - 1) {
    forecastData[currentIndex] = trendData[currentIndex].rate
  }

  // 根据视图类型设置标签
  const yAxisName = isRegionView ? 'Unemployment Rate (%)' : 'Demand Rate'
  const seriesName = isRegionView ? 'Unemployment Rate' : 'Demand Rate'

  const option: echarts.EChartsOption = {
    tooltip: {
      trigger: 'axis',
      formatter: (params: any) => {
        if (!params || params.length === 0) return ''

        // 找到第一个有值的数据
        const validData = params.find((p: any) => p.value !== null && p.value !== undefined)
        if (!validData || validData.dataIndex === undefined) return ''

        const dataItem = trendData[validData.dataIndex]
        if (!dataItem) return ''

        // 判断是否为预测数据
        const isForecast = String(dataItem.flag) !== '0'
        const suffix = isRegionView ? '%' : ''
        return `${dataItem.year} ${dataItem.month}<br/>${seriesName}: ${validData.value}${suffix}${isForecast ? ' (Forecast)' : ''}`
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
        formatter: (value: number) => {
          return isRegionView ? `${value}%` : `${value}`
        },
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
        // 添加时间标记线
        markLine: {
          silent: false,
          symbol: 'none',
          data: [
            // 当前时间标记线（红色）
            {
              xAxis: currentIndex,
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
            },
            // 用户选择时间标记线（绿色）
            {
              xAxis: trendData.findIndex(
                (item) => item.year === selectedYear.value && item.month === selectedMonth.value,
              ),
              label: {
                show: true,
                position: 'start',
                formatter: 'Selected Time',
                color: '#10b981',
                fontSize: 11,
                fontWeight: 'bold',
              },
              lineStyle: {
                color: '#10b981',
                width: 2,
                type: 'dashed',
              },
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
        // 将地图名称转换为标准地区名称以查找数据
        const mapName = params.name
        const standardRegionName = mapNameToStandardRegionName(mapName)

        // 查找对应的数据（使用标准名称）
        const dataItem = data.find((item) => item.name === standardRegionName)

        if (isRegionView) {
          // Region视图：显示失业率信息
          if (dataItem) {
            return `${mapName}<br/>Unemployment Rate: ${dataItem.value}%<br/>Unemployed: ${dataItem.unemployment_people?.toLocaleString() || 'N/A'}<br/>Total Workforce: ${dataItem.all_people?.toLocaleString() || 'N/A'}`
          } else {
            return `${mapName}<br/>Unemployment Rate: N/A<br/>Unemployed: N/A<br/>Total Workforce: N/A`
          }
        } else {
          // Occupation视图：显示需求率信息
          if (dataItem) {
            return `${mapName}<br/>Demand Rate: ${dataItem.value}<br/>Job Hunters: ${dataItem.job_hunters?.toLocaleString() || 'N/A'}<br/>Job Vacancies: ${dataItem.job_vacancy?.toLocaleString() || 'N/A'}`
          } else {
            return `${mapName}<br/>Demand Rate: N/A<br/>Job Hunters: N/A<br/>Job Vacancies: N/A`
          }
        }
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
            color: '#ffffff',
            fontWeight: 'bold',
            fontSize: 11,
          },
          itemStyle: {
            areaColor: '#60a5fa',
            borderColor: '#ffffff',
            borderWidth: 1,
            shadowBlur: 10,
            shadowColor: 'rgba(0, 0, 0, 0.3)',
          },
        },
        itemStyle: {
          areaColor: '#94a3b8',
          borderColor: '#64748b',
          borderWidth: 1,
        },
        label: {
          show: true,
          fontSize: 10,
          color: '#ffffff',
          fontWeight: '600',
          textBorderColor: 'rgba(0, 0, 0, 0.5)',
          textBorderWidth: 2,
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
      console.log('点击的地区（地图名称）:', params.name)

      // 将地图名称转换为标准地区名称
      const standardRegionName = mapNameToStandardRegionName(params.name)
      console.log('转换后的标准地区名称:', standardRegionName)

      // 如果点击的是同一个地区，则取消选择
      if (selectedRegion.value === standardRegionName) {
        selectedRegion.value = null
      } else {
        // 否则选择新的地区（使用标准名称）
        selectedRegion.value = standardRegionName
      }
    }
  })
}
// Watchers for reactive updates
watch(activeTab, () => {
  nextTick(() => {
    updateChart()
    // 切换tab后,确保地图调整大小以适应新的容器宽度
    setTimeout(() => {
      if (mapChart) {
        mapChart.resize()
      }
    }, 350) // 等待CSS transition完成
  })
})
watch(selectedYear, () => {
  // In a real application, this would filter the data by year
  nextTick(() => {
    updateChart()
    // 如果趋势图已经打开，更新趋势图以反映新的选择时间
    if (selectedRegion.value && lineChart) {
      updateLineChart()
    }
  })
})
watch(selectedMonth, () => {
  // In a real application, this would filter the data by month
  nextTick(() => {
    updateChart()
    // 如果趋势图已经打开，更新趋势图以反映新的选择时间
    if (selectedRegion.value && lineChart) {
      updateLineChart()
    }
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
