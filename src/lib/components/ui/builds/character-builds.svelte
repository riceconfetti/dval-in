<script lang="ts">
	import * as Tabs from '$lib/components/ui/character-tabs';
	import Text from '$lib/components/typography/Text.svelte';
	import Card from '../card/card.svelte';
	import WeaponItem from './weapon-item.svelte';
	import ArtifactItem from './artifact-item.svelte';
	import type { ArtifactSetKey } from '$lib/types/keys/ArtifactSetKey';
	import type { WeaponIndex } from '$lib/types/index/weapon';
	import { isWeaponKey, type WeaponKey } from '$lib/types/keys/WeaponKey';
	import IconCirclet from '$lib/assets/Icon_Circlet_of_Logos.png';
	import IconGoblet from '$lib/assets/Icon_Goblet_of_Eonothem.png';
	import IconSands from '$lib/assets/Icon_Sands_of_Eon.png';

	export let builds: Build[];
	export let weaponIndex: WeaponIndex;

	type Build = {
		name: string;
		description: string[];
		talentPriority: string[];
		weapons: {
			signatureWeapon: string;
			altWeapons: string[];
		};
		artifacts: {
			signatureSet: {
				key: string;
				quantity: number;
			};
			altSets: {
				key: string;
				quantity: number;
			}[];
			stats: {
				main: string[];
				sub: string[];
			};
		};
	};

	function getKey(key: string): ArtifactSetKey | WeaponKey {
		if (isWeaponKey(key)) {
			return <WeaponKey>key;
		} else {
			return <ArtifactSetKey>key;
		}
	}

	const buildItems = [
		{
			name: 'Sands of Eon',
			img: IconSands
		},
		{
			name: 'Goblet of Eonothem',
			img: IconGoblet
		},
		{
			name: 'Circlet of Logos',
			img: IconCirclet
		}
	];
	// [IMPORTANT!] Change IMG links later!!
</script>

<Tabs.Root value={builds[0].name} class="w-full">
	<Tabs.List class={`grid grid-cols-${builds.length}`}>
		{#each builds as build}
			<Tabs.Trigger value={build.name}>{build.name}</Tabs.Trigger>
		{/each}
	</Tabs.List>
	{#each builds as build}
		<Tabs.Content value={build.name}>
			<div class="flex flex-col lg:flex-row gap-4 pt-2">
				<div
					class="flex flex-col lg:order-2 lg:grid lg:grid-cols-2 lg:grid-rows-2 lg:grid-flow-dense gap-4"
				>
					<Card class="h-min max-w-full flex flex-col gap-4 p-4 lg:h-auto">
						<Text type="h3">Weapons</Text>
						<WeaponItem
							signature={true}
							rarity={weaponIndex[getKey(build.weapons.signatureWeapon)].rarity}
							refine={1}
							key={getKey(build.weapons.signatureWeapon)}
							index={weaponIndex}
						/>
						<div class="flex flex-col gap-2">
							{#each build.weapons.altWeapons as alt}
								<WeaponItem
									signature={false}
									rarity={weaponIndex[getKey(alt)].rarity}
									key={getKey(alt)}
									index={weaponIndex}
								/>
							{/each}
						</div>
					</Card>
					<!--Weapon Recomendation Card-->

					<Card class="h-min max-w-full flex flex-col gap-4 p-4 lg:row-span-2">
						<Text type="h3">Artifacts</Text>
						<ArtifactItem
							signature={true}
							rarity={5}
							quantity={build.artifacts.signatureSet.quantity}
							key={getKey(build.artifacts.signatureSet.key)}
						/>
						<div class="flex flex-col gap-2">
							{#each build.artifacts.altSets as alt}
								<ArtifactItem signature={false} rarity={5} key={getKey(alt.key)} />
							{/each}
						</div>
						<Text type="h4">Main Stats</Text>
						<div
							class="grid grid-cols-3 grid-rows-2 grid-flow-col items-center justify-items-center text-center gap-2"
						>
							{#each build.artifacts.stats.main as item, i}
								<img
									class="w-full min-w-0 object-contain h-fit max-w-12"
									alt={buildItems[i].name}
									src={buildItems[i].img}
								/>
								<Text type="p">{item}</Text>
							{/each}
						</div>

						<Text type="h4">Sub Stats</Text>
						<div class="flex flex-col gap-1">
							{#each build.artifacts.stats.sub as item}
								<Text type="p">{item}</Text>
							{/each}
						</div>
					</Card>
					<!--Artifact Recomendation Card-->

					<Card class="h-min max-w-full flex flex-col gap-4 p-4">
						<Text type="h3">Talents</Text>
						<div class="flex flex-row flex-1 gap-2 h-16 justify-center items-center">
							<img
								class="w-full min-w-0 object-contain max-w-12"
								src={buildItems[0].img}
								alt={build.talentPriority[0]}
							/>
							<p>&gt;</p>
							<img
								class="w-full min-w-0 object-contain max-w-12"
								src={buildItems[0].img}
								alt={build.talentPriority[1]}
							/>
							<p>&gt;</p>
							<img
								class="w-full min-w-0 object-contain max-w-12"
								src={buildItems[0].img}
								alt={build.talentPriority[2]}
							/>
						</div>
					</Card>
					<!--Talent Priority Card-->
				</div>
				<div class="basis-3/4 flex flex-col gap-2 lg:order-1">
					{#each build.description as p}
						<Text type="p">
							{p}
						</Text>
					{/each}
				</div>
			</div>
		</Tabs.Content>
	{/each}
</Tabs.Root>
