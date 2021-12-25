<script>
	let m = 69;
	let f = 440;
	let o = 4;
	let n = 'A'
	let notes = ['C','C#','D','Eb','E','F','F#','G','G#','A','Bb','B']
	let octaves = [...Array(8).keys()]
	
	$: setFromMidi(m)
	$: setFromFreq(f)
	$: setFromNoteOctave(n,o)

	// https://newt.phys.unsw.edu.au/jw/notes.html
	function setFromMidi(value) {
		m = +value;
		f = freqFromMidi(m)
		o = octaveFromMidi(m)
		n = noteFromMidi(m)
	}

	function setFromFreq(value) {
		f = +value;
		m = midiFromFreq(f)
		o = octaveFromMidi(m)
		n = noteFromMidi(m)
	}
	
	function setFromNoteOctave(note, octave) {
		o = octave
		n = note
		m = midiFromNoteOctave(note, octave)
		f = freqFromMidi(m)
	}
	
	// https://stackoverflow.com/questions/712679/convert-midi-note-numbers-to-name-and-octave
	function octaveFromMidi(m) {
		return Math.floor((m / 12) - 1);	
	}
	
	function noteFromMidi(m) {
		return notes[m % 12]
	}
	
	function midiFromNoteOctave(n, o) {
		return ((o+1)*12)+notes.indexOf(n)
	}
	
	function midiFromFreq(f) {
		return Math.round((12*Math.log2(f/440.0)+69));
	}
	
	function freqFromMidi(m) {
		return (Math.pow(2, (m-69)/12)*440).toFixed(1)
	}
	
</script>

<input bind:value={m} type=number> MIDI 

<br/>

<input bind:value={f} type=number> Hz

<br/>

<select bind:value={n}>
	{#each notes as note}
		<option value={note}>
			{note}
		</option>
	{/each}
</select>

<select bind:value={o}>
	{#each octaves as octave}
		<option value={octave}>
			{octave}
		</option>
	{/each}
</select>

<style>
	input {
		width: 8em;
	}
</style>