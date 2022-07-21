<script>
	import Map from '../components/map.svelte';
	import { data } from '$lib/data.js';

	let selectedPlot = '033';
	$: selected = data[selectedPlot];

	import { Disclosure, DisclosureButton, DisclosurePanel } from '@rgossiaux/svelte-headlessui';
	import { ChevronRightIcon } from '@rgossiaux/svelte-heroicons/solid';

	import numeral from 'numeral';

	try {
		numeral.register('locale', 'en-full', {
			delimiters: {
				thousands: ',',
				decimal: '.'
			},
			abbreviations: {
				thousand: 'thousand',
				million: 'million',
				billion: 'billion',
				trillion: 'trillion'
			},
			ordinal: function (number) {
				var b = number % 10;
				return ~~((number % 100) / 10) === 1
					? 'th'
					: b === 1
					? 'st'
					: b === 2
					? 'nd'
					: b === 3
					? 'rd'
					: 'th';
			},
			currency: {
				symbol: '$'
			}
		});
	} catch (error) {
		console.error(error);
	}

	numeral.locale('en-full');

	const placeholder = 'No data';

	$: generalData = selected
		? [
				{
					title: 'Formation',
					value: selected.formation || placeholder
				},
				{
					title: 'Population',
					value: numeral(selected.population).format() || placeholder
				},
				{
					title: 'Birthright Citizenship',
					value: selected.birthright || placeholder
				},
				{
					title: 'Official Language(s)',
					value: selected.languages || placeholder
				},
				{
					title: 'Land Area',
					value: selected.area ? `${numeral(selected.area).format()} km²` : placeholder
				}
		  ]
		: null;

	$: economicData = selected
		? [
				{
					title: 'Currency',
					value: selected.currency || placeholder
				},
				{
					title: 'GDP (nominal)',
					value: selected.gdp ? `\$${numeral(selected.gdp).format('0.00 a')}` : placeholder
				},
				{
					title: 'Main Industries',
					value: selected.industries || placeholder
				},
				{
					title: 'Oil Production',
					value: selected.oil || placeholder
				},
				{
					title: 'Coal Mining',
					value: selected.coal || placeholder
				}
		  ]
		: null;

	$: militaryData = selected
		? [
				{
					title: 'Military Spending',
					value: selected.militarySpending
						? `${selected.militarySpending.toFixed(1)}% of GDP`
						: placeholder
				},
				{
					title: 'Conscription',
					value: selected.conscription || placeholder
				},
				{
					title: 'Nuclear Weapons',
					value: selected.nuclearWeapons || placeholder
				},
				{
					title: 'Biological Weapons',
					value: selected.biologicalWeapons || placeholder
				},
				{
					title: 'Chemical Weapons',
					value: selected.chemicalWeapons || placeholder
				}
		  ]
		: null;

	$: governmentData = selected
		? [
				{
					title: 'Source of Power',
					value: selected.powerSource || placeholder
				},
				{
					title: 'Ideology',
					value: selected.ideology || placeholder
				},
				{
					title: 'Legislature',
					value: selected.legislature.name || placeholder
				},
				...(selected.legislature.upperHouse
					? [
							{
								title: 'Upper House',
								value: selected.legislature.upperHouse
							}
					  ]
					: []),
				...(selected.legislature.lowerHouse
					? [
							{
								title: 'Lower House',
								value: selected.legislature.lowerHouse
							}
					  ]
					: []),
				{
					title: 'Hate Speech',
					value: selected.hateSpeech || placeholder
				},
				{
					title: 'Life Imprisonment',
					value: selected.lifeImprisonment || placeholder
				},
				{
					title: 'Torture',
					value: selected.torture || placeholder
				},
				{
					title: 'Capital Punishment',
					value: selected.capitalPunishment || placeholder
				},
				{
					title: 'Same-sex Marriage',
					value: selected.sameSexMarriage || placeholder
				},
				{
					title: 'Prostitution',
					value: selected.prostitution || placeholder
				},
				{
					title: 'Lane Splitting',
					value: selected.laneSplitting || placeholder
				},
				{
					title: 'Right to Internet Access',
					value: selected.internetAccess || placeholder
				},
				{
					title: 'Drugs',
					value: selected.drugs || placeholder
				},
				{
					title: 'Universal Healthcare',
					value: selected.healthcare || placeholder
				},
				{
					title: 'Vaccination',
					value: selected.vaccination || placeholder
				},
				{
					title: 'Abortion',
					value: selected.abortion || placeholder
				}
		  ]
		: null;

	$: developmentData = selected
		? [
				{
					title: 'HDI',
					value:
						selected.lifeExpectancy && selected.mys && selected.eys && selected.GNIpc
							? numeral(
									Math.pow(
										(((((selected.lifeExpectancy - 20) / 65) *
											(selected.mys / 15 + selected.eys / 18)) /
											2) *
											(Math.log(selected.GNIpc) - Math.log(100))) /
											(Math.log(75000) - Math.log(100)),
										1 / 3
									)
							  ).format('0.000')
							: placeholder
				},
				{
					title: 'Life Expectancy',
					value: selected.lifeExpectancy
						? numeral(selected.lifeExpectancy).format('0.0')
						: placeholder
				},
				{
					title: 'Mean Years of Schooling',
					value: selected.mys ? numeral(selected.mys).format('0.0') : placeholder
				},
				{
					title: 'Expected Years of Schooling',
					value: selected.eys ? numeral(selected.eys).format('0.0') : placeholder
				},
				{
					title: 'GNI per capita',
					value: selected.GNIpc ? numeral(selected.GNIpc).format('$0,0') : placeholder
				}
		  ]
		: null;

	$: miscData = selected
		? [
				{
					title: 'Most Popular Sport',
					value: selected.sport || placeholder
				},
				{
					title: 'Maximum Speed Limit',
					value: selected.speedLimit || placeholder
				},
				{
					title: 'Driving Side',
					value: selected.drivingSide || placeholder
				}
		  ]
		: null;

	$: dataGroups = [
		{
			title: 'General',
			data: generalData
		},
		{
			title: 'Economic',
			data: economicData
		},
		{
			title: 'Military',
			data: militaryData
		},
		{
			title: 'Government and Politics',
			data: governmentData
		},
		{
			title: 'Development',
			data: developmentData
		}
	];
</script>

<div class="flex flex-row h-screen gap-6 p-6">
	<div class="flex items-center flex-1">
		<div class="flex-1 border rounded-lg shadow">
			<Map bind:selectedPlot />
		</div>
	</div>
	<div class="flex-1 space-y-4 overflow-y-scroll rounded-lg">
		{#if selected}
			<div
				class="overflow-hidden bg-gray-800 bg-center bg-no-repeat bg-cover rounded-lg shadow-md"
				style="background-image:url('{selected.flag}')"
			>
				<div class="bg-gray-900/40 backdrop-blur-sm h-60 flex items-end p-4 text-white rounded-lg">
					<div>
						{#if selected.pretitle}
							<h4 class="text-white/50 sm:text-xl md:text-2xl text-lg font-medium">
								The <span class="text-white/75 font-bold">{selected.pretitle}</span> of
							</h4>
						{/if}
						<h3 class="sm:text-3xl md:text-4xl text-2xl font-extrabold">
							{selected.name}
						</h3>
						{#if selected.governmentType}
							<p class="text-white/80 sm:text-base text-sm font-medium">
								{selected.governmentType}
							</p>
						{/if}
					</div>
				</div>
			</div>

			{#each dataGroups as { title, data }}
				<Disclosure let:open>
					<DisclosureButton
						class="hover:bg-blue-100 focus:outline-none focus-visible:ring focus-visible:ring-blue-500 focus-visible:ring-opacity-75 bg-blue-50 flex w-full p-2 text-lg font-medium text-left text-blue-900 transition duration-150 ease-in-out rounded-lg"
					>
						<ChevronRightIcon
							class={`${
								open ? 'transform rotate-90' : ''
							} w-7 h-7 text-blue-500 transition duration-150 ease-in-out`}
						/>
						<span>{title}</span>
					</DisclosureButton>
					<DisclosurePanel
						class="sm:grid-cols-3 gap-x-2 sm:gap-y-2 md:text-base grid grid-cols-1 p-4 mt-2 text-sm bg-gray-100 rounded-lg"
					>
						{#each data as { title, value }}
							<div class="sm:text-right text-gray-400">
								{title}
							</div>
							<div class="sm:col-span-2 sm:mb-0 col-span-2 mb-2">
								{value}
							</div>
						{/each}
					</DisclosurePanel>
				</Disclosure>
			{/each}
		{:else if selectedPlot}
			<h3 class="text-3xl font-extrabold">
				<span class="text-gray-600">Plot</span>
				{selectedPlot}
			</h3>
			<p>No data is available for this plot.</p>
		{:else}
			<h3 class="text-3xl font-extrabold">No plot selected</h3>
			<p>Click on a plot on the map to view available data.</p>
		{/if}
	</div>
</div>
